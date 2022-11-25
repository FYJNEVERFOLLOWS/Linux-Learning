[#]: subject: "How to Record Streaming Audio in Ubuntu and other Linux Distributions"
[#]: via: "https://itsfoss.com/record-streaming-audio/"
[#]: author: "Abhishek Prakash https://itsfoss.com/"
[#]: collector: "lkxed"
[#]: translator: "FYJNEVERFOLLOWS"
[#]: reviewer: " "
[#]: publisher: " "
[#]: url: " "

如何在 Ubuntu 和其他 Linux 发行版中录制流音频
======
如何在 Ubuntu 和其他 Linux 发行版中录制音频？

如果你想通过计算机的麦克风录制语音，可以使用 GNOME Sound Recorder 或 Audacity。

使用 GNOME Sound Recorder 很简单，但它缺乏功能。Audacity 最初可能会让人无法抗拒，但它有很多专业级录音的功能。然而，在本教程中，我不会详细讨论这个问题。

GNOME Sound Recorder 能与麦克风配合使用。还有一个叫做 Audio recorder 的工具，除了麦克风输入，你可以使用它来录制流媒体音乐（来自 Sptify、YouTube、互联网广播、Skype 和其他大多数来源）。

总而言之，我将向你展示以下步骤：

* 使用 GNOME Sound Recorder 录制声音
* 使用 Audio Recorder 录制流音频

### 使用 Sound Recorder 从麦克风录制音频

GNOME 桌面环境有很多有用的应用程序。Sound Recorder 就是其中之一。

你可以从 Ubuntu 软件中心安装 [Sound Recorder][1]。

![Sound Recorder can be installed from the Ubuntu Software Center][2]

或者，你可以在终端中使用此命令来安装它：

```
sudo apt install gnome-sound-recorder
```

安装后，你可以在系统菜单中找到它并从那里开始。

![GNOME Sound Recorder][3]

在开始使用它之前，应确保在系统设置中选择了正确的输入源

![Ensure that you have chosen correct input in system settings][4]

一打开 Sound Recorder，它将显示如下界面。

![Hit the Record button to start audio recording][5]

点击录制按钮，它立即开始录制音频。录制时，你可以选择暂停、停止或取消录制。

![Options while recording audio][6]

你的录音将保存并可从应用程序界面本身获得。单击保存的录音以突出显示。

你可以回放或删除录音。你可以通过单击保存/下载按钮选择将其保存到其他位置。你也可以使用编辑按钮重命名录音。

![Saved recordings][7]

这很方便，对吧？你可以选择以 MP3、FLAC 和多种格式录制。

#### 删除 GNOME Sound Recorder 

不喜欢它或发现它缺乏功能？

你可以从 Ubuntu 软件中心删除 GNOME Sound Recorder 或使用以下命令：

```
sudo apt remove gnome-sound-recorder
```

GNOME Sound Recorder 的应用受到限制。它只从麦克风录制，在某些情况下这不是你想要的。

想象一下你想录制 Skype 通话或在应用程序或网络浏览器中播放的内容？在这种情况下，漂亮的 Audio Recorder 会有所帮助。

### 使用 Audio Recorder 来录制流音频

You can watch this video to see how to use Audio Recorder. It’s a bit old but the steps are the same.

![A Video from YouTube][8]

[Subscribe to our YouTube channel for more Linux videos][9]

You can use the [official PPA][10] to install Audio Recorder in Ubuntu and Linux Mint. Use the following commands in the terminal (Ctrl+Alt+T) one by one:

```
sudo apt-add-repository ppa:audio-recorder/ppa
sudo apt update
sudo apt install audio-recorder
```

Alternatively, you can download the source code from [launchpad][11]. Once installed, you can start the application from the Activity Overview:

![Audio Recorder][12]

#### Record all kinds of sound from various sources

Audio Recorder records all kinds of sounds your computer makes.

It records audio played through your system’s soundcard, microphones, browsers, webcams and more.

In other words, it records even if your system sneezes (given that you want to record it). It allows you to select the recording device such as webcam, microphone, Skype, etc.

To record the streaming music, select the appropriate source. For example, if you are playing streaming radio in Rhythmbox, then select Rythmbox.

![Audio-Recorder Audio Settings][13]

#### Record at your convenience

Audio Recorder also gives you the option of setting timer. You can start, stop or pause recording at a given clock time or at a pre-defined interval. You can also set the limit on the recorded file size.

Moreover, you can pause (and stop) when there is no audio (or very low sound) and resume it when sound comes back.

All you have to do is to edit the text in the Timer panel. Comment out the “rules” you don’t want to apply and edit the ones per your requirement.

![Audio-recorder Timer Settings][14]

It provides additional settings like auto start at login, show tray icon and other record settings.

![Audio-recorder Additional Settings][15]

#### Save the recorded music file in various file formats

Another gem. You can save the recorded file in your favourite file format. Supported file formats are OGG audio, Flac, MP3, SPX and WAV. I prefer MP3 for my recordings.

The **recorded files are stored in ~/Audio** i.e., in the Audio folder inside your home directory.

![Audio-recorder Audio Formats][16]

#### How good is Audio Recorder?

I used Audio Recorder in Ubuntu to [record the music played on YouTube][17]. I saved a 2-minute video in MP3 format that took 934 KB of space. But I must say I was not expecting the recorded sound quality to be so good. Honestly, I could not distinguish it from the original YouTube song.

#### Removing Audio Recorder

If you don’t find Audio Recorder to your liking, you can remove it using the following commands:

```
sudo apt remove audio-recorder
```

It will be a good idea to [remove the PPA as well][18]:

```
sudo apt-add-repository -r ppa:audio-recorder/ppa
```

### Conclusion

There are probably several other tools for audio recording in Linux. Like GNOME, other desktop environments may also have sound recording apps. I know Deepin has one for sure.

GNOME Sound Recorder is a decent tool for recording sound from your microphone. For recording sound from various sources, Audio Recorder is a good choice.

I hope it helps with your audio recording needs. Let me know if you have any suggestions.

--------------------------------------------------------------------------------

via: https://itsfoss.com/record-streaming-audio/

作者：[Abhishek Prakash][a]
选题：[lkxed][b]
译者：[译者ID](https://github.com/FYJNEVERFOLLOWS)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://itsfoss.com/
[b]: https://github.com/lkxed
[1]: https://wiki.gnome.org/Apps/SoundRecorder
[2]: https://itsfoss.com/wp-content/uploads/2022/08/sound-recorder-ubuntu.png
[3]: https://itsfoss.com/wp-content/uploads/2022/08/sound-recorder.png
[4]: https://itsfoss.com/wp-content/uploads/2022/08/microphone-settings-ubuntu.png
[5]: https://itsfoss.com/wp-content/uploads/2022/08/using-sound-recorder-linux.png
[6]: https://itsfoss.com/wp-content/uploads/2022/08/sound-recording-with-sound-recorder.png
[7]: https://itsfoss.com/wp-content/uploads/2022/08/sound-recorder-interface.png
[8]: https://youtu.be/o7Ia2QGeB7Q
[9]: https://www.youtube.com/c/itsfoss?sub_confirmation=1
[10]: https://launchpad.net/~audio-recorder/+archive/ubuntu/ppa
[11]: https://launchpad.net/audio-recorder
[12]: https://itsfoss.com/wp-content/uploads/2022/08/audio-recorder-in-overview.png
[13]: https://itsfoss.com/wp-content/uploads/2022/08/audio-recorder-audio-settings.png
[14]: https://itsfoss.com/wp-content/uploads/2022/08/audio-recorder-timer-settings.png
[15]: https://itsfoss.com/wp-content/uploads/2022/08/audio-recorder-additional-settings.png
[16]: https://itsfoss.com/wp-content/uploads/2022/08/audio-recorder-audio-formats.png
[17]: https://itsfoss.com/youtube-dl-audio-only/
[18]: https://itsfoss.com/how-to-remove-or-delete-ppas-quick-tip/
