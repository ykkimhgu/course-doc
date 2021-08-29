# Installing MDK uVision

## Installation

### Download 

Go to keil.com and Download **MDK-Lite \(MDK-Core\)** : [click here](https://www2.keil.com/mdk5) 

> v5.35  @ 2021.8

![](../.gitbook/assets/image%20%2816%29.png)

### Install uVision

Start installation. 

> This lecture uses mdk535.exe

![](../.gitbook/assets/image%20%281%29.png)

Pack directory는 default로 설정 \(경로에 한글이름 없게, 띄어쓰기 지양\)

![](../.gitbook/assets/image%20%286%29.png)

It will ask for Ulink driver installation. Proceed with installation

> Keil Tools 범용 직렬버스 컨트롤러 설치\(USB 연결 드라이브\)

![](../.gitbook/assets/image%20%2840%29.png)



### Install Packs

After the uVision IDE is installed,  we need to install necessary software Packs.

We are going to use STM32F411RE board for the lab, and we will install additional software packs specific for this board.

> If you use STM32F401RE, then search for this model

On Left Window Pane:  **Devices** tab:  **STMicroelectronics &gt; STM32F4 Series&gt; STM32F411RE&gt; STM32F411RETx** 선택 

On Right Window Pane: Packs tab: **Keil::STM32F4xxFxx\_DFP**  Install

![](../.gitbook/assets/image%20%2825%29.png)

### Install ST-Link Driver

[Download driver \(STSW-LINK009\) here](https://www.st.com/en/development-tools/stsw-link009.html)

> ST-LINK, ST-LINK/V2, ST-LINK/V2-1, STLINK-V3 USB driver signed for Windows10. This USB driver \(STSW-LINK009\) is for ST-LINK/V2, ST-LINK/V2-1 and STLINK-V3 boards\(STM8/STM32 discovery boards, STM8/STM32 evaluation boards and STM32 Nucleo boards\). 
>
> It declares to the system the USB interfaces possibly provided by the ST-LINK: ST Debug, Virtual COM port and ST Bridge interfaces. **The driver must be installed prior to connecting the device, in order to have a successful enumeration.**

After you download  "**en.stsw-link009\_v2.0.2.zip**",  unzip the file.

Update the driver by 

1. Connect MCU to PC
2. 윈도우  장치 관리자 &gt; 범용직렬버스장치&gt;  ST-Link Debug &gt;드라이버 업데이트&gt;  Select  en-stsw-link009 unzipped folder

![](../.gitbook/assets/image%20%283%29.png)

> What is ST Link utiliy?
>
> [https://m.blog.naver.com/ansdbtls4067/221510252896](https://m.blog.naver.com/ansdbtls4067/221510252896)



