<properties 
    pageTitle="Täpsemad koos Media kodeerija Standard kodeerimine" 
    description="Selles teemas kirjeldatakse, kuidas teha Täpsem kodeerimine kohandada Media kodeerija Standard tööülesande valmiskombinatsioonid. Teema näitab, kuidas Media Services .NET SDK abil saate luua tööülesande kodeering ja töö. See ka näitab, kuidas kohandatud valmiskombinatsioonid kodeering töö esitama." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Täpsemad koos Media kodeerija Standard kodeerimine

##<a name="overview"></a>Ülevaade

Selles teemas kirjeldatakse, kuidas teha Täpsemad kodeerimise ülesannete Media kodeerija Standard. Teema näitab, [Kuidas kasutada .NET kodeering toimingu ja tööd, mis käivitab selle ülesande loomiseks](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Samuti kujutatakse esitama kohandatud valmiskombinatsioonid kodeering tööülesannet. Elemendid, mida kasutatakse valmissätteid kirjelduse leiate teemast [selles dokumendis](https://msdn.microsoft.com/library/mt269962.aspx). 

Järgmisi kodeering ülesandeid valmissätteid on näidanud:

- [Luuakse pisipildid](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [(Lõikamine) video trimmimine](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Ülekatte loomine](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Vaikne heliriba lisada, kui sisendit ei heli](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Keela automaatne de-põimimine](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Ainult heli valmiskombinatsioonid](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Kahe või enama videofaile CONCATENATE](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Media kodeerija Standard Kärbi videod](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Video jälituse lisamine, kui sisendit ei video](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Video pööramine](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Meediumi teenustega .NET SDK kodeerimine

Järgmine kood näide kasutab Media Services .NET SDK teha järgmisi toiminguid:

- Luua mõne kodeering töö.
- Saada Media kodeerija Standard kodeerija viide.
- Kohandatud XML- või JSON valmissäte laadida. Saate salvestada XML-i või JSON (nt [XML](media-services-custom-mes-presets-with-dotnet.md#xml) või [JSON](media-services-custom-mes-presets-with-dotnet.md#json) fail ja kasutage järgmist faili laadimiseks.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Töö kodeering toimingu lisada. 
- Määrake Sisestuskeel varade olema kodeeritud.
- Saate luua väljundi varade, mis sisaldab encoded vara.
- Lisage sündmuseohjuri töö edenemise kontrollimiseks.
- Esitage töö.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
        
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
        
                            // Display or log error details as needed.
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Suhteline tugi

Pisipiltide selle loomisel peate määrama alati väljundi laius ja kõrgus pikslites. Saate need määrata protsentide vahemikus [1%,..., 100%].

###<a name="json-preset"></a>JSON valmissäte 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-i valmissäte
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Luuakse pisipildid

Selles jaotises kirjeldatakse, kuidas kohandada valmissäte, mis on genereeritud pisipilte. Määratletud valmissäte leiate teavet selle kohta, kuidas soovite kodeerida oma faili kui ka pisipildid loomiseks vajalik teave. Saate teha mõnda MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx) ja lisada genereeritud pisipilte vöötkood.  

>[AZURE.NOTE]Järgmised eelseadistatud saab ainult **SceneChangeDetection** säte seatud väärtuse true, kui teil on kodeering ühe video bitrate. Kui teil on mitme bitikiirusega video kodeerimine ja **SceneChangeDetection** väärtuseks true, tagastab funktsioon kodeerija tõrke.  


Skeemi kohta leiate teavet teemast [selle](https://msdn.microsoft.com/library/mt269962.aspx) teema.

Veenduge, et jaotises [peaksite arvesse võtma](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>JSON valmissäte


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-i valmissäte


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Kaalutlused

Kehtivad järgmisega.

- Start/samm/vahemik konkreetsete ajatemplid kasutamine eeldab, et sisendandmete allikas on vähemalt 15 minutit.
- Jpg/Png/BmpImage elemendid on Alusta etappi, ja vahemiku stringi atribuudid – saate tõlgendada nii:

    - Raami Number, kui need on negatiivne täisarvud, näiteks "Start": "120"
    - Suhteline andmeallika kestus juhul, kui nii % tähe, näiteks "Start": "15%", või
    - Kui väljendatud HH:MM:SS ajatempli... vormindamine, näiteks "Start": "00: 01:00"

    Saate segada ja märgete nagu palun vastavad.
    
    Lisaks toetab Start ka teisiti makro: {parimad}, mis avaldab esimese "huvitav" paneeli sisu teate määratlemiseks: (samm ja vahemiku ignoreeritakse alustamine on valitud {kõige paremini})
    
    - Vaikesätted: Alustamine: {parim}
- Väljundvorming tuleb esitada konkreetselt iga pildivorming: Jpg/Png/BmpFormat. Kui see on olemas, MES vastab JpgVideo, et JpgFormat jne. OutputFormat tutvustab uus pilt-kodekiga kindla makro: {Index}, mis peab olema esitamine (üks kord ja ainult üks kord) pilt väljundvormingusse.

##<a id="trim_video"></a>(Lõikamine) video trimmimine

Selles jaotises räägib kodeerija valmiskombinatsioonid lõikepiltide või kus on nn Standard faili või nõudmisel faili Sisestuskeel video trimmimine muutmine. Lõikepiltide või trimmimine vara, mis on hõivatud või arhiivitakse reaalajas voo kaudu saab ka kodeerija – see üksikasjad on saadaval [selles](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)blogis.

Trimmimise videoid, saate võtta MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx) ja muuta element **allikatest** (nagu allpool näidatud). StartTime väärtus peab vastama absoluutne ajatemplid Sisestuskeel video. Näiteks kui esimese Sisestuskeel video kaadri on ajatempel 12:00:10.000, siis StartTime peaks olema vähemalt 12:00:10.000 ja suuremad. Alltoodud näites me oletame, et Sisestuskeel video on käivitamine ajatempli null. **Allikate** tuleks paigutada valmissäte alguses. 
 
###<a id="json"></a>JSON valmissäte
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-i valmissäte
    
Trimmimise videoid, saate võtta MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx) ja muuta element **allikatest** (nagu allpool näidatud).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Ülekatte loomine

Media kodeerija Standard võimaldab ülekattega pildi peale olemasoleva video. Praegu on toetatud failivormingud: png, jpg-, gif-, bmp- ja. Määratletud valmissäte on lihtne näide video ülekatet.

Lisaks määratlemine loodud fail, peate ka lasta Media Servicesi teada, kus faili vara on ülekatet pilt ja milline on allika video peale, mille soovite ülekattega pilt. Videofaili peab olema **esmane** fail. 

Ülaltoodud näites .NET määratleb kaks funktsiooni: **UploadMediaFilesFromFolder** ja **EncodeWithOverlay**. Funktsiooni UploadMediaFilesFromFolder (nt BigBuckBunny.mp4 ja Image001.png) kausta failide üleslaadimine ja määrab mp4 faili olema vara esmane fail. Funktsiooni **EncodeWithOverlay** kasutab kohandatud loodud fail, mille sellele (nt algne, millele järgneb) edastati kodeering ülesande loomiseks. 

>[AZURE.NOTE]Praeguse piirangud:
>
>Ülekatet läbipaistmatust sätet ei toetata.
>
>Video lähtefaili ja ülekatet pildifaili peavad olema sama vara ja videofaili tuleb määrata selle vara esmane fail.

###<a name="json-preset"></a>JSON valmissäte
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-i valmissäte
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Vaikne heliriba lisada, kui sisendit ei heli

Kui saadate kodeerija, mis sisaldab ainult video ja heli ei sisend siis väljundi varade sisaldab vaikimisi failid, mis sisaldavad ainult video andmeid. Mõned mängijad ei saa käsitlemiseks sellise väljundi voogu. Selle sätte abil saate jõustamine vaikne heliriba lisamiseks väljundi selle stsenaariumi kodeerija.

Jõustamine kodeerija andes varade, mis sisaldab vaikne heliriba kui sisendit ei heli, määrake väärtus "InsertSilenceIfNoAudio".

Saate võtta MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx)ja pärast muudatusi teha.

###<a name="json-preset"></a>JSON valmissäte

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-i valmissäte

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Keela automaatne de-põimimine

Klientide pole vaja midagi teha, kui ta saaks automaatselt tühistage põimitud interlace sisu. Kui soovitud automaatne tühistage põimimine on sisse lülitatud (vaikesäte) Evakuatsioonisüsteemi ei põimitud raamid automaatne avastamine ja ainult tühistage interlaces raamid interlaced märgitud.

Saate selle automaatne tühistage põimimine välja lülitada. See suvand pole soovitatav.

###<a name="json-preset"></a>JSON valmissäte
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-i valmissäte
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Ainult heli valmiskombinatsioonid

Selles jaotises näitab kahe ainult MES valmiskombinatsioonid: AAC heli- ja AAC hea kvaliteediga heli.

###<a name="aac-audio"></a>AAC heli 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC kvaliteetse heli

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Kahe või enama videofaile CONCATENATE

Järgmises näites on näidatud, kuidas luua valmissäte kasutaksite kahe või enama videofaile. Kõige levinum stsenaarium on, kui soovite lisada päise- või kesktelghaagise peamised video. Otstarbe on kui redigeerib koos videofailid ühiskasutus atribuudid (video eraldusvõime, raami määr, heliriba arv jne). Teil peaks Olge ettevaatlik, et mitte segada videod paneeli erinevate määrade või muu heli radade arv.

###<a name="requirements-and-considerations"></a>Nõuded ja kaalutlused

- Sisestuskeel videod peaks olema ainult üks heliriba.
- Sisestuskeel videod peaks olema sama paneeli kiirus.
- Peate eraldi varade videote üleslaadimine ja seadmine videod iga vara esmane fail.
- Peate teadma videote kestus.
- Algne alltoodud näiteid eeldab, et kõik Sisestuskeel videod alustamine ajatempel null. Peate muutma StartTime väärtused, kui videod on erinevate käivitamine ajatempli, nagu see on tavaliselt reaalajas arhiivi.
- JSON valmissäte teeb konkreetsed viited Sisestuskeel vara AssetID väärtuse.
- Proovi kood eeldab, et JSON valmissäte on salvestatud kohalik fail, näiteks "C:\supportFiles\preset.json". Eeldatakse, et kahe varad on loodud kaks video failide üleslaadimise ja teate, AssetID saadud väärtused.
- Koodilõigu ja JSON valmissäte kujutab näidet, ühendades kahe videofaile. Saate selle laiene rohkem kui kaks Videod:

    1. Tööülesande helistamine. Mitu korda tabeldusklahvi, et lisada rohkem videoid, et InputAssets.Add().
    2. Tehes vastavad muudatused "Allikad" elemendi JSON, rohkem kirjeid, lisades samas järjestuses. 


###<a name="net-code"></a>.Net-i kood

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>JSON valmissäte

Värskendage oma kohandatud eelmääratud varade, mida soovite concatenate ID-d ja iga video lõigu sobival ajal.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Media kodeerija Standard Kärbi videod

Vaadake teemat [Kärbi videod Media kodeerija standardne](media-services-crop-video.md) .

##<a id="no_video"></a>Video jälituse lisamine, kui sisendit ei video

Kui saadate kodeerija, mis sisaldab ainult heli ja video pole sisend siis väljundi varade sisaldab vaikimisi failid, mis sisaldavad ainult heli andmed. Mõned mängijad, sh Azure Media Player (vt teemat [selle](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) ei pruugi käsitlema selliste voogu. Selle sätte abil saate jõustada kodeerija mustvalge video jälituse lisamiseks väljundi seda stsenaariumi. 

>[AZURE.NOTE]Sunnib kodeerija lisamiseks on väljund video jälituse suureneb väljund suurust varade, ja seetõttu maksumus tekkinud kodeering ülesande jaoks. Peaks töötama teste veendumaks, et tulemuseks suurendamine on ainult mõõdukas mõju teie kuutasu.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Ainult madalam bitrate veebisaidil video lisamine

Oletame, et kasutate on mitu bitrate kodeerimine eelmääratud nagu ["H264 mitme Bitrate 720p"](https://msdn.microsoft.com/library/mt269960.aspx) kodeerida kogu Sisestuskeel kataloogi streaming, mis sisaldab video faile ja ainult failid. Sel juhul, kui sisend on pole video, võite jõustamine kodeerija lisamiseks mustvalge video jälituse ainult madalam bitrate, erinevalt veebisaidil iga väljundi bitrate video lisamine. Selleks, peate määrama "InsertBlackIfNoVideoBottomLayerOnly" lippu.

Saate võtta MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx)ja pärast muudatusi teha.

#### <a name="json-preset"></a>JSON valmissäte

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-i valmissäte

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Video lisamine üldse väljund bitikiirus

Oletame, et kasutate on mitu bitrate kodeerimine eelmääratud nagu ["H264 mitme Bitrate 720p](https://msdn.microsoft.com/library/mt269960.aspx) kodeerida kogu Sisestuskeel kataloogi streaming, mis sisaldab video faile ja ainult failid. Selle stsenaariumi korral kui sisend on pole video, võite lisada mustvalge video Jälita üldse väljundi bitikiirus kodeerija jõustamine. Tagatakse, et teie väljundi varad on kõik ühtlast arvu jälitab video ja heli rajad suhtes. Selleks, peate määrama "InsertBlackIfNoVideo" lippu.

Saate võtta MES valmiskombinatsioonid dokumenteeritud [siin](https://msdn.microsoft.com/library/mt269960.aspx)ja pärast muudatusi teha.

#### <a name="json-preset"></a>JSON valmissäte

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-i valmissäte
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Video pööramine

Funktsiooni [Media kodeerija Standard](media-services-dotnet-encode-with-media-encoder-standard.md) toetab nurgad pöördenurka 0, 90, 180 ja 270. Vaikekäitumine on "Automaatne", kui püüab tuvasta pööre metaandmete sissetuleva video fail ja klõpsake seda. Järgmised järgmistest **allikatest** elemendi ühele valmiskombinatsioonid määratletud [siin](http://msdn.microsoft.com/library/azure/mt269960.aspx).

### <a name="json-preset"></a>JSON valmissäte

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-i valmissäte

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Vaadake lisateavet [selle](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) teema kohta, kuidas kodeerija tõlgendab laius ja kõrgus sätted on valmiskombinatsiooni pööre hüvitamise käivitamisel.

Saate näitamaks, et ignoreerida pööre metaandmed, kui see on olemas, siis sisendit video kodeerija väärtuse "0". 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka 

[Media Services kodeering ülevaade](media-services-encode-asset.md)
