---
title: 'Qwen3-TTS Steps Up: Voice Cloning and Voice Design!'
link: https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign
source: qwen-ai-blog
published: 2025-12-22T16:00:45Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.html
preview:
  file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.preview-878b4eb485a8.webp
  width: 256
  height: 154
  color: '#d1abeb'
images:
- source: https://img.alicdn.com/imgextra/i4/O1CN01Q9UPif1FfSgpkSBnP_!!6000000000514-0-tps-1890-1134.jpg
  original:
    file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-d142b520703d.webp
    width: 1890
    height: 1134
  variants:
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-63d192f305d0.webp
    width: 48
    height: 29
  color: '#d9a6ea'
- source: http://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3-TTS-1211/tablevd.png
  original:
    file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-cfc13a2b1865.png
    width: 3840
    height: 1265
  variants:
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-6bfebae6294f.webp
    width: 48
    height: 16
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-4584ca691c30.webp
    width: 320
    height: 105
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-eca8ccc79530.webp
    width: 640
    height: 211
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-191698d04fa8.webp
    width: 960
    height: 316
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-f3e2e22ab099.webp
    width: 1280
    height: 422
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-25ae938e80d2.webp
    width: 1600
    height: 527
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-b8f09a233d67.webp
    width: 3840
    height: 1265
  color: '#fbfbfb'
- source: http://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3-TTS-1211/tablevc.png
  original:
    file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-163587fec765.png
    width: 4143
    height: 1265
  variants:
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-ae4d316e654b.webp
    width: 48
    height: 15
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-87b4ba51e899.webp
    width: 320
    height: 98
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-0d0394ce1973.webp
    width: 640
    height: 195
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-1d028be81b75.webp
    width: 960
    height: 293
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-708579fd94be.webp
    width: 1280
    height: 391
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-200dc088f9d5.webp
    width: 1600
    height: 489
  - file: 2025-12-22-qwen3-tts-steps-up-voice-cloning-and-voice-design.image-067468eae8d5.webp
    width: 4143
    height: 1265
  color: '#fbfbfb'
---

[Qwen3-TTS-VD-Flash HF DEMO](https://huggingface.co/spaces/Qwen/Qwen3-TTS-Voice-Design) [Qwen3-TTS-VD-Flash MODELSCOPE DEMO](https://modelscope.cn/studios/Qwen/Qwen3-TTS-Voice-Design) [Qwen3-TTS-VC-Flash HF DEMO](https://huggingface.co/spaces/Qwen/Qwen-TTS-Clone-Demo) [Qwen3-TTS-VC-Flash MODELSCOPE DEMO](https://modelscope.cn/studios/Qwen/Qwen-TTS-Clone-Demo)

**Qwen3-TTS** family has launched two new models: the voice design model Qwen3-TTS-VD-Flash (accessible via the [Qwen API](https://www.alibabacloud.com/help/en/model-studio/qwen-tts-voice-design)) and the voice cloning model Qwen3-TTS-VC-Flash (accessible via the [Qwen API](https://www.alibabacloud.com/help/en/model-studio/qwen-tts-voice-cloning)).

Key Features:

- **Voice Design**：Qwen3-TTS-VD-Flash supports complex natural language instructions, enabling fine-grained control over timbre, prosody, emotion, persona, and more, achieving full control from “what to say” to “how to say it.” It allows users to freely define the desired voice, completely freeing them from only being able to clone existing voices or choose from a limited set of preset voices. On InstructTTS-Eval, it significantly outperforms GPT-4o-mini-tts and Mimo-audio-7b-instruct overall, and surpasses Gemini-2.5-pro-preview-tts in role-playing tests.

- **Voice Cloning**：Qwen3-TTS-VC-Flash supports 3-second voice cloning, and can generate speech in 10 major languages—Chinese, English, German, Italian, Portuguese, Spanish, Japanese, Korean, French, and Russian—based on the cloned voice. On the MiniMax TTS Multilingual Test Set, its average word error rate (WER) is consistently better than MiniMax, ElevenLabs, and GPT-4o-Audio-Preview.

- **High Expressiveness**：Qwen3-TTS-VD-Flash and Qwen3-TTS-VC-Flash offer highly expressive, humanlike voices that can stably and reliably produce speech closely aligned with the input text, automatically adjusting tone and rhythm according to semantic content for natural and vivid delivery.

- **Robust Text Handling**：Qwen3-TTS-VD-Flash and Qwen3-TTS-VC-Flash have strong text parsing capabilities, automatically handling complex text structures and accurately extracting key information, showing strong robustness when dealing with diverse and non-standard text formats.

\
\

Your browser does not support the video tag.

## Qwen3-TTS-VD-Flash [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#qwen3-tts-vd-flash)

Qwen3-TTS supports creating customized voice profiles directly from natural language descriptions. Users can freely describe acoustic attributes, persona settings, background information, and more, making it easy to create the exact kind of voice they want.

### Metrics [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#metrics)

Controllable generation: On the InstructTTS-Eval benchmark, Qwen3-TTS significantly outperforms GPT-4o-mini-tts and Mimo-audio-7b-instruct overall, and surpasses Gemini-2.5-pro-preview-tts on role‑playing tests.

![](http://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3-TTS-1211/tablevd.png#center)

### Samples [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#samples)

| Control Type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Control Instruction                                                                                                                                                                                                                                                                                                                                              | Text                                                                                                                                                            | Samples |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Acoustic attribute: positive/negative                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 模仿电视购物主持人，中年男性，声音洪亮有激情，语速极快，音调夸张上扬，用极具煽动性的语气来介绍产品，营造出紧迫感和抢购氛围。                                                                                                                                                                                                                                                                                                   | 不要九百九十八，也不要八百八十八，今天只要九十八！对，你没有听错，只要九十八！赶快拿起电话订购吧！                                                                                                               |         |
| Male, middle-aged, booming baritone - hyper-energetic infomercial voice with rapid-fire delivery and exaggerated pitch rises, dripping with salesmanship                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Not nine hundred ninety-nine dollars! Not seven hundred ninety-nine! today, it's just $98! That's right, you heard me, ONLY NINETY-EIGHT DOLLARS! Don't wait, don't hesitate—pick up the phone and call NOW!                                                                                                                                                     |                                                                                                                                                                 |         |
| 展现出悲苦沙哑的声音质感,语速偏慢,情绪浓烈且带有哭腔,以标准普通话缓慢诉说,情感强烈,语调哀怨高亢,音高起伏大。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 这些年代受的苦，就跟你说上十天半个月也说不完。                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                 |         |
| Male, 30s, strained tenor - breathy sobs interrupt speech, pitch swings wildly between whispers and wails                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The suffering I've endured... I could talk for days, weeks even, and still not scratch the surface.                                                                                                                                                                                                                                                              |                                                                                                                                                                 |         |
| Persona role-play: concise/rich                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 邪恶女魔头                                                                                                                                                                                                                                                                                                                                                            | 哥哥，你回来啦，人家等了你好久好久了，要抱抱！                                                                                                                                         |         |
| Playful Homebody Sis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Big brooo, you're finally home! I've been waiting forever! Gimme a hug, pwease~!                                                                                                                                                                                                                                                                                 |                                                                                                                                                                 |         |
| 角色姓名：陈远山 身份背景：某国家重点科研项目首席顾问，年近七十的资深战略科学家。曾参与国家重大科技攻关工程，历经数十年风雨，见证了从落后追赶到自主创新的艰难历程。现任国家科技咨询委员会终身荣誉委员，仍坚持在一线培养青年人才，为国家战略发展建言献策。 外貌特征：身形挺拔，两鬓斑白，眉宇间刻着岁月沉淀的坚毅。常着深色中山装或简洁正装，眼神沉静而锐利，举手投足间自带威严与从容。 性格特质：意志如钢，信念坚定，面对挑战从不退缩；胸怀家国，心系民族未来，将个人命运与国家兴衰紧密相连；严谨自律，言出必行，话语中充满责任感与历史担当；外冷内热，表面严肃，实则对后辈寄予厚望，甘为人梯。 人生信条：“我们这一代人，不是为了站在光里，而是为了把路铺到光里。”                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 有些事，只要国家需要，就得有人扛起来。 我们那一代人，是背着泥土铺路的； 你们要做的，是让这条路，通向星辰大海。                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                 |         |
| role: Mid-level Corporate Project Manager. gender: Male. pitch: Dynamic male pitch, starting mid-high with agitation, transitioning to a lower declarative range, and spiking upwards with intense emphasis such as 'so finished!'. speed: Variable speaking rate; initially rapid during agitated states ('Damn it!'), slowing for declarative statements like 'I'm done', then accelerating again with strong emotional delivery ('so finished!').. volume: Significant dynamic range; initially loud and forceful, briefly softening to a firm conversational level, then escalating to shouting at points of high emphasis like 'so finished!'. age: Middle-aged adult. clarity: Consistently clear articulation, maintained even during rapid or loud emotional expressions.. fluency: Fluent and coherent speech, with pauses and pacing that align with the expressed emotional state.. accent: General American English. texture: Predominantly forceful, becoming strained during agitated outbursts and shouting, otherwise resonant and firm during calmer declarations.. emotion: Starts with pronounced frustration and exasperation ('Damn it!'), shifts to resolute decisiveness ('I'm done'), culminating in an intensely emphatic declaration of finality ('I am so finished!').. tone: Begins as agitated and questioning, transitions to assertive and declarative, and concludes with a highly emphatic and intense quality.. personality: Assertive and emotionally expressive, demonstrating a build-up of frustration leading to a decisive, forceful resolution.. | So am I damn it. I mean, come on. It's just, you know what I'm through with this. I'm done. Finished, I'm out of here. I am so finished. Those were coming when the health.                                                                                                                                                                                      |                                                                                                                                                                 |         |
| Background information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 《少年闰土》是节选自鲁迅1921年写的短篇小说《故乡》中的一段插叙， 主人公以鲁迅的童年伙伴章运水为原型。 《少年闰土》的题目是被选入小学语文教材后编者加的，自1980年起，它基本上一直保留在小学语文教材中， 目前被选入小学语文教材统编版六年级上册。 《少年闰土》以回忆的方式展开， 刻画出一个机敏勇敢、见多识广的闰土形象。 全文按照“记忆—相识—相处—相别”的顺序书写， 依次介绍了“我”记忆中看瓜刺猹的闰土、初次相识时的闰土、给“我”讲新鲜事的闰土， 少年闰土给“我”的童年生活带来了无穷的新奇与乐趣。 作者善用白描手法，多用直接引语， 其中又蕴含丰富的情感，既有“我”在相见之前对闰土的盼望，也有相处过程中对闰土的羡慕和向往，还有分别时的依依不舍。在短短的相处中，“我”和闰土彼此之间结下深厚情谊。 | 深蓝的天空中挂着一轮金黄的圆月，下面是海边的沙地，种着一望无际的碧绿西瓜。其间十一二岁的少年闰土，项带银圈，手捏钢叉，向一匹猹刺去。那猹却将身一扭，反从他胯下逃走——这幅月夜刺猹的画面，成了“我”三十年来难忘的剪影。那年，因家中轮到三十余年一遇的大祭祀值年，祭器贵重需人看管，父亲便允了忙月的请求，唤其子闰土进城相助。 |         |
| "Do not go gentle into that good night" is a poem in the form of a villanelle by Welsh poet Dylan Thomas (1914–1953), and is one of his best-known works. Though first published in the journal Botteghe Oscure in 1951, Thomas wrote the poem in 1947 while visiting Florence with his family. The poem was subsequently included, alongside other works by Thomas, in In Country Sleep, and Other Poems (New Directions, 1952) and Collected Poems, 1934–1952 (Dent, 1952). The poem entered the public domain in all countries outside the United States on 1 January 2024. It has been suggested that the poem was written for Thomas's dying father, although he did not die until just before Christmas in 1952. It has no title other than its first line, "Do not go gentle into that good night", a line that appears as a refrain throughout the poem along with its other refrain, "Rage, rage against the dying of the light".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Do not go gentle into that good night Old age should burn and rave at close of day Rage, rage against the dying of the light Though wise men at their end know dark is right Because their words had forked no lightning They do not go gentle into that good night                                                                                              |                                                                                                                                                                 |         |

Users can also persistently store and repeatedly invoke the voices created by Qwen3-TTS, enabling the generation of vivid and natural multi-turn, multi-role long-form dialogues.

| Control Type                                                                                                                                                                                                                                                            | Control Instruction                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Text                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Samples |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Voice reuse                                                                                                                                                                                                                                                             | "旁白": "声音特征沉稳、客观、略带叙事感的女播音腔，普通话标准，语速适中，带有轻微的环境氛围渲染，语调平缓但富有感染力，在关键情节时稍作停顿，增强画面感。情感冷静旁观，偶尔带一丝微妙的反讽" "小林": "25岁男性上班族，声音清亮但时常犹豫，语速时快时慢，紧张时会轻微结巴。情绪波动明显，从低声呢喃到突然激动再到自我怀疑的叹气。肢体语言丰富，经常无意识的小动作" "御姐": "模拟成熟性感的御姐音色，声音略带磁性且沉稳，语速不快不慢，语调充满自信和一丝挑逗，尾音可以稍微拖长并上扬，给人一种游刃有余的掌控感。"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 旁白: 小林今天第三次走神了。酒吧昏黄的灯光晃得他心跳加速，而吧台对面那个红唇微扬的女人，正用指尖轻轻摩挲着酒杯边缘。 御姐: 小弟弟，有兴趣陪姐姐喝一杯吗？ 小林: 啊？我、我……我其实不太会喝酒…… 旁白: 他的手指无意识地抠着杯沿，喉结上下滚动，像被什么无形的东西掐住了呼吸。 御姐: 不会喝？那正好——姐姐教你。这杯莫吉托，甜得刚好，就像你刚才偷看我的眼神。 小林: 我、我没偷看！……好吧，看了一眼。就一眼！ 旁白: 他猛地坐直，又立刻缩回肩膀，仿佛那句话烫伤了自己的嘴。 御姐: 紧张什么？你连坐姿都在发抖……要不要靠过来一点？这里太吵了。 小林: 靠过去？可、可我们才第一次见面……你都不认识我…… 御姐: 名字不重要，感觉才重要。......而我感觉……你有点可爱。 旁白: 小林的耳朵瞬间红透，连耳后那颗小痣都像在发烫。他想逃，脚却像钉在了高脚凳上。 小林: 可爱？没人这么说过我……他们都说我太闷，连朋友圈都发不出手…… 御姐: 那现在呢？敢不敢发一条——'今晚，和一个危险又迷人的姐姐喝了一杯'？ 小林: ……我连配图都不敢选。你笑起来太……太有杀伤力了。 御姐: 那就别发了。有些故事，只适合藏在两个人的记忆里——比如，接下来你打算请我跳支舞吗？ 旁白: 他张了张嘴，没发出声音。但这一次，他没有低头，而是轻轻推开了那杯没动过的苏打水，朝她伸出了手。 |         |
| "Lucas": "Male, 17 years old, tenor range, gaining confidence - deeper breath support now, though vowels still tighten when nervous" "Mia": "Female, 16 years old, mezzo-soprano range, softening - lowering register to intimate speaking voice, consonants softening" | Lucas:H-hey! You dropped your... uh... calculus notebook? I mean, I think it's yours? Maybe? Mia:Oh wow, my mortal enemy - Mr. Thompson's problem sets. Thanks for rescuing me from that F. Lucas:No problem! I actually... kinda finished those already? If you want to compare answers or something... Mia:Is this your sneaky way of saying you want to study together, Lucas? Because I saw you staring during lab partners sign-up. Lucas:What? No! I mean yes but not like... I just think you're... your titration technique is really precise! Mia:That's the nerdiest compliment I've ever gotten. Tell you what - help me survive pre-calc and I'll teach you how to actually flirt. Lucas:Wow, harsh. And here I thought my titration line was smooth. Mia:It was adorable. Like when you tripped over your shoelaces in the hall yesterday. Or that time you— Lucas:Okay okay! I get it, I'm a disaster. So... library after school? I'll bring the graphing calculators? Mia:Only if you promise not to spill coffee on my notes again... though I guess watching you panic-clean was pretty cute. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |         |

### How to use [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#how-to-use)

Using Qwen3-TTS-VD-Flash via the Qwen API is very simple. Below is a short code snippet to try it out:

```python
import requests
import base64
import os

def create_voice_and_play():
    # API keys differ between Singapore and Beijing regions. Get your API key: https://www.alibabacloud.com/help/zh/model-studio/get-api-key
    # If you haven't set an environment variable, replace the line below with: api_key = "sk-xxx"
    api_key = os.getenv("DASHSCOPE_API_KEY")
    
    if not api_key:
        print("Error: DASHSCOPE_API_KEY environment variable not found. Please set your API key.")
        return None, None, None
    
    # Prepare request data
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }
    
    data = {
        "model": "qwen-voice-design",
        "input": {
            "action": "create",
            "target_model": "qwen3-tts-vd-realtime-2025-12-16",
            "voice_prompt": "A composed middle-aged male announcer with a deep, rich and magnetic voice, a steady speaking speed and clear articulation, is suitable for news broadcasting or documentary commentary.",
            "preview_text": "Dear listeners, hello everyone. Welcome to the evening news.",
            "preferred_name": "announcer",
            "language": "en"
        },
        "parameters": {
            "sample_rate": 24000,
            "response_format": "wav"
        }
    }
    
    # URL for Singapore region. For Beijing region, use: https://dashscope.aliyuncs.com/api/v1/services/audio/tts/customization
    url = "https://dashscope-intl.aliyuncs.com/api/v1/services/audio/tts/customization"
    
    try:
        # Send request
        response = requests.post(
            url,
            headers=headers,
            json=data,
            timeout=60  # Add timeout setting
        )
        
        if response.status_code == 200:
            result = response.json()
            
            # Get voice name
            voice_name = result["output"]["voice"]
            print(f"Voice name: {voice_name}")
            
            # Get preview audio data
            base64_audio = result["output"]["preview_audio"]["data"]
            
            # Decode Base64 audio data
            audio_bytes = base64.b64decode(base64_audio)
            
            # Save audio file locally
            filename = f"{voice_name}_preview.wav"
            
            # Write audio data to local file
            with open(filename, 'wb') as f:
                f.write(audio_bytes)
            
            print(f"Audio saved to local file: {filename}")
            print(f"File path: {os.path.abspath(filename)}")
            
            return voice_name, audio_bytes, filename
        else:
            print(f"Request failed. Status code: {response.status_code}")
            print(f"Response: {response.text}")
            return None, None, None
            
    except requests.exceptions.RequestException as e:
        print(f"Network request error: {e}")
        return None, None, None
    except KeyError as e:
        print(f"Response format error: missing required field: {e}")
        print(f"Response: {response.text if 'response' in locals() else 'No response'}")
        return None, None, None
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None, None, None

if __name__ == "__main__":
    print("Creating voice...")
    voice_name, audio_data, saved_filename = create_voice_and_play()
    
    if voice_name:
        print(f"\nSuccessfully created voice '{voice_name}'")
        print(f"Audio file saved: '{saved_filename}'")
        print(f"File size: {os.path.getsize(saved_filename)} bytes")
    else:
        print("\nVoice creation failed")
```

## Qwen3-TTS-VC-Flash [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#qwen3-tts-vc-flash)

Qwen3-TTS supports natural, 3‑second–level voice cloning, and can generate multilingual audio based on the cloned voice. It is also highly robust when handling complex text and in-the-wild audio.

### Metrics [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#metrics-1)

Multilingual voice cloning: On the MiniMax TTS Multilingual Test Set, Qwen3‑TTS shows more stable content than MiniMax, ElevenLabs, and GPT‑4o‑Audio‑Preview for Chinese, English, French, Italian, and other languages, achieving the best average word error rate (WER).

![](http://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3-TTS-1211/tablevc.png#center)

### Samples [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#samples-1)

| Cloning Type                                                                                                                                                                   | Reference Audio                                                                                                                                                                        | Text                                                                                               | Samples |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ------- |
| Chinese–English cloning                                                                                                                                                        |                                                                                                                                                                                        | 昨夜雨疏风骤，浓睡不消残酒。试问卷帘人，却道海棠依旧。知否，知否？应是绿肥红瘦。                                                           |         |
| Overcome with guilt, Martin hung his head and muttered, "I’m so sorry. I never meant to hurt you like this. Can you ever forgive me?" It was obvious what the answer would be. |                                                                                                                                                                                        |                                                                                                    |         |
|                                                                                                                                                                                | 再说，学好文化搞通思想，道理还不是为了劳动？难道我劳动比谁差！                                                                                                                                                        |                                                                                                    |         |
| Innovation blossoms when we cast aside the paralyzing fear of failure and wholeheartedly embrace the gloriously messy, unexpectedly beautiful journey of creation.             |                                                                                                                                                                                        |                                                                                                    |         |
| Multilingual cloning                                                                                                                                                           |                                                                                                                                                                                        | 上周我去日本旅游，看到一个法国人在买东西，那个日本人问：なんか買うものありますか？那个法国人说：Je voudrais acheter un t-shirt à manches courtes.' |         |
|                                                                                                                                                                                | 呐，跟你说个秘密哦！The tea eggs sold by the old lady on the mountaintop are something else, I tell you! Es wird durch Einkochen mit Quellwasser und drei verschiedenen Wildkräutern zubereitet. |                                                                                                    |         |
| Robustness on complex text                                                                                                                                                     |                                                                                                                                                                                        | Qwen-TTS 是支持音色克隆、生成、控制的语音合成模型，不仅支持多语言multilingual，还支持各种复杂文本，如pin1 yin1，特殊符号等·〛』］；能读出各种生僻字词。快来试试吧！  |         |
|                                                                                                                                                                                | Is there anyone who can solve the equation: x = \[-b ± √(b²-4ac)\] / 2a? Nobody can — it's a disaster, very sad! If you know this formula, please email solution@prize.org.            |                                                                                                    |         |
| Robustness on in-the-wild audio                                                                                                                                                |                                                                                                                                                                                        | 妲己凭借着自己的妖娆妩媚，在商纣王的宫廷中弄权，她的行为可谓是牝鸡司晨，加速了商朝的灭亡                                                       |         |
|                                                                                                                                                                                | We began our discussion on the four development phases of romantic relationships by reading a quote from 'The General Theory of Love.'                                                 |                                                                                                    |         |

Want to hear how animals would sound if they could talk? Qwen3-TTS can show you cross-species cloning:

| Cloning Type          | Reference Audio                                                                      | Text                            | Samples |
| --------------------- | ------------------------------------------------------------------------------------ | ------------------------------- | ------- |
| Cross-species cloning |                                                                                      | 这点小事都能办砸？说好晚上七点准时开饭，本汪的肚子都咕咕叫了！ |         |
|                       | That was one small leap for me, but a giant leap for goatkind!                       |                                 |         |
|                       | 早起的鸟儿有虫吃，早起的虫儿被我吃！                                                                   |                                 |         |
|                       | Oink... not now. My mud nap is at peak fluffiness. Disturb me and I’ll snore louder. |                                 |         |

### How to use [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#how-to-use-1)

Using Qwen3-TTS-VC-Flash via the Qwen API is very simple. Below is a short code snippet to try it out:

```python
# The DashScope SDK version must be 1.23.9 or later, and the Python version must be 3.10 or later.
# coding=utf-8
# Installation instructions for pyaudio:
# APPLE Mac OS X
#   brew install portaudio
#   pip install pyaudio
# Debian/Ubuntu
#   sudo apt-get install python-pyaudio python3-pyaudio
#   or
#   pip install pyaudio
# CentOS
#   sudo yum install -y portaudio portaudio-devel && pip install pyaudio
# Microsoft Windows
#   python -m pip install pyaudio

import pyaudio
import os
import requests
import base64
import pathlib
import threading
import time
import dashscope  # The DashScope Python SDK version must be 1.23.9 or later.
from dashscope.audio.qwen_tts_realtime import QwenTtsRealtime, QwenTtsRealtimeCallback, AudioFormat

# ======= Constant configuration =======
DEFAULT_TARGET_MODEL = "qwen3-tts-vc-realtime-2025-11-27"  # The same model must be used for voice cloning and speech synthesis.
DEFAULT_PREFERRED_NAME = "guanyu"
DEFAULT_AUDIO_MIME_TYPE = "audio/mpeg"
VOICE_FILE_PATH = "voice.mp3"  # The relative path of the local audio file for voice cloning.

TEXT_TO_SYNTHESIZE = [
    'Right? I really like this kind of supermarket,',
    'especially during the New Year.',
    'Going to the supermarket',
    'just makes me feel',
    'super, super happy!',
    'I want to buy so many things!'
]

def create_voice(file_path: str,
                 target_model: str = DEFAULT_TARGET_MODEL,
                 preferred_name: str = DEFAULT_PREFERRED_NAME,
                 audio_mime_type: str = DEFAULT_AUDIO_MIME_TYPE) -> str:
    """
    Create a voice and return the voice parameter.
    """
    # The API keys for the Singapore and Beijing regions are different. To get an API key, see https://www.alibabacloud.com/help/en/model-studio/get-api-key.
    # If you have not configured the environment variable, replace the following line with your Model Studio API key: api_key = "sk-xxx"
    api_key = os.getenv("DASHSCOPE_API_KEY")

    file_path_obj = pathlib.Path(file_path)
    if not file_path_obj.exists():
        raise FileNotFoundError(f"The audio file does not exist: {file_path}")

    base64_str = base64.b64encode(file_path_obj.read_bytes()).decode()
    data_uri = f"data:{audio_mime_type};base64,{base64_str}"

    # The following is the URL for the Singapore region. If you use a model in the Beijing region, replace the URL with: https://dashscope.aliyuncs.com/api/v1/services/audio/tts/customization
    url = "https://dashscope-intl.aliyuncs.com/api/v1/services/audio/tts/customization"
    payload = {
        "model": "qwen-voice-enrollment", # Do not modify this value.
        "input": {
            "action": "create",
            "target_model": target_model,
            "preferred_name": preferred_name,
            "audio": {"data": data_uri}
        }
    }
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }

    resp = requests.post(url, json=payload, headers=headers)
    if resp.status_code != 200:
        raise RuntimeError(f"Failed to create the voice: {resp.status_code}, {resp.text}")

    try:
        return resp.json()["output"]["voice"]
    except (KeyError, ValueError) as e:
        raise RuntimeError(f"Failed to parse the voice response: {e}")

def init_dashscope_api_key():
    """
    Initialize the API key for the DashScope SDK.
    """
    # The API keys for the Singapore and Beijing regions are different. To get an API key, see https://www.alibabacloud.com/help/en/model-studio/get-api-key.
    # If you have not configured the environment variable, replace the following line with your Model Studio API key: dashscope.api_key = "sk-xxx"
    dashscope.api_key = os.getenv("DASHSCOPE_API_KEY")

# ======= Callback class =======
class MyCallback(QwenTtsRealtimeCallback):
    """
    Custom TTS streaming callback.
    """
    def __init__(self):
        self.complete_event = threading.Event()
        self._player = pyaudio.PyAudio()
        self._stream = self._player.open(
            format=pyaudio.paInt16, channels=1, rate=24000, output=True
        )

    def on_open(self) -> None:
        print('[TTS] Connection established')

    def on_close(self, close_status_code, close_msg) -> None:
        self._stream.stop_stream()
        self._stream.close()
        self._player.terminate()
        print(f'[TTS] Connection closed code={close_status_code}, msg={close_msg}')

    def on_event(self, response: dict) -> None:
        try:
            event_type = response.get('type', '')
            if event_type == 'session.created':
                print(f'[TTS] Session started: {response["session"]["id"]}')
            elif event_type == 'response.audio.delta':
                audio_data = base64.b64decode(response['delta'])
                self._stream.write(audio_data)
            elif event_type == 'response.done':
                print(f'[TTS] Response complete, Response ID: {qwen_tts_realtime.get_last_response_id()}')
            elif event_type == 'session.finished':
                print('[TTS] Session finished')
                self.complete_event.set()
        except Exception as e:
            print(f'[Error] Exception occurred while processing callback event: {e}')

    def wait_for_finished(self):
        self.complete_event.wait()

# ======= Main execution logic =======
if __name__ == '__main__':
    init_dashscope_api_key()
    print('[System] Initializing Qwen TTS Realtime ...')

    callback = MyCallback()
    qwen_tts_realtime = QwenTtsRealtime(
        model=DEFAULT_TARGET_MODEL,
        callback=callback,
        # The following is the URL for the Singapore region. If you use a model in the Beijing region, replace the URL with: wss://dashscope.aliyuncs.com/api-ws/v1/realtime
        url='wss://dashscope-intl.aliyuncs.com/api-ws/v1/realtime'
    )
    qwen_tts_realtime.connect()
    
    qwen_tts_realtime.update_session(
        voice=create_voice(VOICE_FILE_PATH), # Replace the voice parameter with the custom voice generated by cloning.
        response_format=AudioFormat.PCM_24000HZ_MONO_16BIT,
        mode='server_commit'
    )

    for text_chunk in TEXT_TO_SYNTHESIZE:
        print(f'[Send text]: {text_chunk}')
        qwen_tts_realtime.append_text(text_chunk)
        time.sleep(0.1)

    qwen_tts_realtime.finish()
    callback.wait_for_finished()

    print(f'[Metric] session_id={qwen_tts_realtime.get_session_id()}, '
          f'first_audio_delay={qwen_tts_realtime.get_first_audio_delay()}s')
```

## Citation [#](https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign#citation)

If you find our model useful in your research, please consider citing us 📝 :)

```bibtex
@misc{qwen3_tts_202512,
  author       = {Qwen Team, Alibaba},
  title        = {Qwen3-TTS Steps Up: Voice Cloning and Voice Design!},
  year         = {2025},
  url          = {https://qwen.ai/blog?id=qwen3-tts-vc-voicedesign},
  urldate      = {2025-12-23}
}
```
