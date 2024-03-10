# 使用Web音频API
- [文档地址](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)
- 让我们来看看如何开始使用Web Audio API。我们将简要介绍一些概念，然后研究一个简单的Boombox示例，该示例允许我们加载音轨、播放和暂停音轨，并更改其音量和立体声平移。
- Web Audio API不会取代<audio>media元素，而是对其进行补充，就像<canvas>与<img>元素共存一样。您的用例将决定您使用什么工具来实现音频。如果您想控制音轨的播放，那么<audio> media元素提供了比Web Audio API更好、更快的解决方案。如果您想执行更复杂的音频处理以及播放，Web Audio API提供了更多的功能和控制。
- Web Audio API的一个强大功能是它没有严格的“声音调用限制”。例如，一次没有32或64个声音呼叫的上限。有些处理器可能能够同时播放1,000多个声音而不结巴。
# 示例代码
- [在Codepen上查看最终的演示](https://codepen.io/Rumyra/pen/qyMzqN/)
- [在GitHub上查看源代码](https://github.com/mdn/webaudio-examples/tree/main/audio-basics)
# 音频图
- Web Audio API中的所有内容都基于音频图的概念，音频图由节点组成。
- Web Audio API在音频上下文中处理音频操作，并被设计为允许模块化路由。基本的音频操作是通过音频节点来执行的，这些音频节点被链接在一起以形成音频路由图。您有输入节点，它是您正在操纵的声音的来源;修改节点，它根据需要更改这些声音;以及输出节点（目标），它允许您保存或收听这些声音。
- 支持具有不同通道布局的多个音频源，即使在单个上下文中也是如此。由于这种模块化设计，您可以创建具有动态效果的复杂音频功能。
# 音频上下文
- 为了能够使用Web Audio API做任何事情，我们需要创建音频上下文的实例。这样我们就可以访问API的所有特性和功能。
    - `const audioContext = new AudioContext();`
    - 注意：如果您只想处理音频数据，例如，缓冲和流式传输，但不播放它，您可能需要创建一个OfflineAudioContext。
# 加载声音
- 现在，我们创建的音频上下文需要一些声音来播放。有几种方法可以使用API来实现这一点。让我们开始与一个简单的方法-因为我们有一个boombox，我们最有可能要发挥一个完整的歌曲轨道。此外，为了便于访问，最好在DOM中公开该曲目。我们将使用<audio>元素在页面上公开歌曲。
    - `<audio src="myCoolTrack.mp3"></audio>`
    - 注意：如果您正在加载的声音文件保存在不同的域中，则需要使用crossorigin属性;有关详细信息，请参阅跨域资源共享（CORS）
- 为了使用我们从Web Audio API中获得的所有好东西，我们需要从这个元素中获取源代码，并将其通过管道传输到我们创建的上下文中。幸运的是，有一种方法可以让我们做到这一点-AudioContext.createMediaElementSource
    ```javascript
    // get the audio element
    const audioElement = document.querySelector("audio");
    // pass it into the audio context
    const track = audioContext.createMediaElementSource(audioElement);
    ```
- 注意：上面的<audio>元素在DOM中由一个类型为HTMLMediaElement的对象表示，它有自己的一组功能。所有这些都保持不变;我们只是允许声音可用于Web Audio API。
# 控制声音
- 在网络上播放声音时，允许用户控制声音是很重要的。根据使用情况，有无数的选项，但我们将提供播放/暂停声音，改变音轨音量以及从左向右平移的功能。
- 从JavaScript代码以编程方式控制声音是由浏览器的自动播放支持策略所涵盖的，因为这样可能会在没有用户授予权限（或允许列表）的情况下被阻止。自动播放策略通常需要明确的权限或用户参与页面，然后脚本才能触发音频播放。
- 这些特殊的要求基本上是因为意外的声音可能会令人讨厌和干扰，并可能导致可访问性问题。您可以在我们的[文章媒体和Web音频API的自动播放指南](https://developer.mozilla.org/en-US/docs/Web/Media/Autoplay_guide)中了解更多信息。
- 由于我们的脚本播放音频以响应用户输入事件（例如，单击播放按钮），因此我们的状态良好，应该不会出现自动播放阻塞的问题。那么，让我们首先来看看我们的播放和暂停功能。我们有一个播放按钮，在播放曲目时会变为暂停按钮
```html
<button data-playing="false" role="switch" aria-checked="false">
  <span>Play/Pause</span>
</button>
```
- 在播放音轨之前，我们需要将音频图从音频源/输入节点连接到目的地。
- 我们已经通过将audio元素传递到API中创建了一个输入节点。在大多数情况下，你不需要创建一个输出节点，你只需要将其他节点连接到BaseAudioContext.destination，它会为你处理这种情况：
    - `track.connect(audioContext.destination);`
- 可视化这些节点的一个好方法是绘制一个音频图，这样你就可以可视化它
- 现在我们可以添加播放和暂停功能
```javascript
// Select our play button
const playButton = document.querySelector("button");

playButton.addEventListener(
  "click",
  () => {
    // Check if context is in suspended state (autoplay policy)
    if (audioContext.state === "suspended") {
      audioContext.resume();
    }

    // Play or pause track depending on state
    if (playButton.dataset.playing === "false") {
      audioElement.play();
      playButton.dataset.playing = "true";
    } else if (playButton.dataset.playing === "true") {
      audioElement.pause();
      playButton.dataset.playing = "false";
    }
  },
  false,
);
```
- 我们还需要考虑当曲目播放结束时该怎么做。我们的HTMLMediaElement在播放结束后触发一个ended事件，所以我们可以监听它并相应地运行代码：
```javascript
audioElement.addEventListener(
  "ended",
  () => {
    playButton.dataset.playing = "false";
  },
  false,
);
```
# 修改声音
- 让我们深入研究一些基本的修改节点，以改变我们的声音。这就是Web Audio API真正开始派上用场的地方。首先，让我们改变音量。这可以使用GainNode来完成，它代表我们的声波有多大。
- 有两种方法可以使用Web音频API创建节点。你可以在上下文本身（例如audioContext.createGain()）或通过节点的构造函数（例如new GainNode()）使用工厂方法。我们将在代码中使用工厂方法：
    - `const gainNode = audioContext.createGain();`
- 现在我们必须更新之前的音频图，因此输入连接到增益，然后增益节点连接到目的地
    - `track.connect(gainNode).connect(audioContext.destination);`
- 增益的默认值为1;这将保持当前音量不变。增益可以设置为最小值约为-3.4028235E38，最大值约为3.4028235E38（JavaScript中的浮点数范围）。在这里，我们将允许boombox将增益提高到2（原始音量的两倍），然后降低到0（这将有效地静音）。
- 让我们给予用户控件来完成此操作-我们将使用范围输入
    - `<input type="range" id="volume" min="0" max="2" value="1" step="0.01" />`
    - 注意：范围输入是一种非常方便的输入类型，用于更新音频节点上的值。您可以指定范围的值，并将它们直接与音频节点的参数一起使用。
- 因此，让我们获取此输入的值，并在输入节点的值被用户更改时更新增益值
```javascript
const volumeControl = document.querySelector("#volume");

volumeControl.addEventListener(
  "input",
  () => {
    gainNode.gain.value = volumeControl.value;
  },
  false,
);
```
- 注意：节点对象的值（例如GainNode.gain）不是简单的值;它们实际上是类型AudioParam的对象-这些称为参数。这就是为什么我们必须设置GainNode.gain的value属性，而不是直接设置gain的值。这使它们更加灵活，例如，允许传递参数一组特定的值，以便在一段时间内进行更改。
- 很好，现在用户可以更新曲目的音量了！如果要添加静音功能，则增益节点是最佳选择
# 将立体声平移添加到我们的应用程序
- 让我们添加另一个修改节点来练习我们刚刚学习的内容
- 有一个StereoPannerNode节点，如果用户有立体声功能，它会改变左右扬声器之间的声音平衡
- 注：StereoPannerNode用于简单的情况，即您只想从左到右进行立体声平移。 还有一个PannerNode，它允许对3D空间或声音空间化进行大量控制，以创建更复杂的效果。 这在游戏和3D应用程序中用于创建飞过头顶的鸟，或者来自用户身后的声音。
- 这次让我们使用构造函数方法来创建一个节点。当我们这样做时，我们必须传入上下文和特定节点可能采取的任何选项：
```javascript
const pannerOptions = { pan: 0 };
const panner = new StereoPannerNode(audioContext, pannerOptions);
```
- 注意：目前并非所有浏览器都支持创建节点的构造函数方法。旧的工厂方法得到了更广泛的支持。
- 这里我们的值范围是-1（最左边）和1（最右边）。再次让我们使用范围类型输入来改变这个参数
    - `<input type="range" id="panner" min="-1" max="1" value="0" step="0.01" />`
- 我们使用该输入中的值来调整panner值，方法与之前相同
```javascript
const pannerControl = document.querySelector("#panner");

pannerControl.addEventListener(
  "input",
  () => {
    panner.pan.value = pannerControl.value;
  },
  false,
);
```
- 让我们再次调整音频图，将所有节点连接在一起
    - `track.connect(gainNode).connect(panner).connect(audioContext.destination);`
# 总结
- 好极了！我们有一个boombox播放我们的'磁带'，我们可以调整音量和立体声平移，给我们一个相当基本的工作音频图。
- 这构成了您开始向网站或Web应用程序添加音频所需的一些基础知识。Web音频API还有很多功能，但一旦您掌握了节点的概念并将音频图放在一起，我们就可以继续查看更复杂的功能。
# 多个示例
- [Voice-change-O-matic](https://github.com/mdn/webaudio-examples/tree/main/voice-change-o-matic)是一个有趣的语音操纵器和声音可视化Web应用程序，允许您选择不同的效果和可视化。该应用程序相当初级，但它演示了多个Web Audio API功能的同时使用。
    - [运行Voice-change-O-matic live](https://mdn.github.io/webaudio-examples/voice-change-o-matic/)
- 另一个专门为演示Web Audio API而开发的应用程序是[Violent Theremin](https://mdn.github.io/webaudio-examples/violent-theremin/)，这是一个简单的Web应用程序，允许您通过移动鼠标指针来更改音高和音量。
    - 它还提供了一个迷幻的灯光秀[见暴力特雷门源代码](https://github.com/mdn/webaudio-examples/tree/main/violent-theremin)。
- 
