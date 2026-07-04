# Arduino：无源蜂鸣器播放音乐



## 说明

用无源蜂鸣器演奏《小星星》旋律。学习 `tone()` 频率输出函数、音符与频率的对应关系、节拍控制、数组存储乐谱的技巧。



## 硬件需求

- Arduino UNO ×1

- 无源蜂鸣器 ×1

- 可选：NPN 三极管（放大声音）



## 电路连接

蜂鸣器正极接 D8，负极接 GND。（如用三极管驱动可更响亮）



## 代码

```cpp

const int BUZZER_PIN = 8;



// 音符频率表（Hz）

#define NOTE_C4  262

#define NOTE_D4  294

#define NOTE_E4  330

#define NOTE_F4  349

#define NOTE_G4  392

#define NOTE_A4  440

#define NOTE_B4  494

#define NOTE_C5  523



// 小星星旋律 {音符, 节拍}

int melody[][2] = {

  {NOTE_C4, 4}, {NOTE_C4, 4}, {NOTE_G4, 4}, {NOTE_G4, 4},

  {NOTE_A4, 4}, {NOTE_A4, 4}, {NOTE_G4, 2},             // 两只老虎

  {NOTE_F4, 4}, {NOTE_F4, 4}, {NOTE_E4, 4}, {NOTE_E4, 4},

  {NOTE_D4, 4}, {NOTE_D4, 4}, {NOTE_C4, 2},

};

int melodyLen = sizeof(melody) / sizeof(melody[0]);



void setup() {

  pinMode(BUZZER_PIN, OUTPUT);

}



void loop() {

  for (int i = 0; i < melodyLen; i++) {

    int note = melody[i][0];

    int duration = 1000 / melody[i][1]; // 节拍转毫秒



    tone(BUZZER_PIN, note, duration);    // 播放音符

    delay(duration * 1.3);              // 音符间隔（130% 留空隙）

    noTone(BUZZER_PIN);                 // 停止发声

  }



  delay(2000);  // 演奏完等2秒再重来

}

```



## 教学重点

- `tone(pin, frequency, duration)`：在指定引脚输出指定频率的方波

- `noTone(pin)`：停止发声

- `sizeof(melody) / sizeof(melody[0])`：自动计算数组长度

- 二维数组存储旋律的编程范式



## 常见错误

- 用了有源蜂鸣器（只有一路频率）→ 换无源蜂鸣器

- `tone()` 与 PWM 冲突：不能用同一个定时器的引脚

- 节拍计算错误导致音乐跑调

- 音符之间没有 `noTone()` 导致连音

