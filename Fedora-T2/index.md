# 인텔 T2 맥북을 Fedora 개발서버로 | Fedora on a T2 Mac

**한국어·English, 5편.** `MacBookPro16,1`에 Fedora KDE를 설치하고 외부 화면, 절전, 원격 데스크톱과 팬 제어를 확인한 기록. 1편은 Ventoy USB로 설치한 뒤 내부 SSD에서 Fedora가 부팅되지 않아, 여유 공간이 168KiB였던 EFI에서 누락된 부팅 파일을 복구한 이야기다.

Five articles about one Intel MacBook Pro. Part 1 follows the Ventoy installation and the recovery of missing boot files on an internal EFI partition with only 168 KiB free. Parts 2–5 cover display stalls, sleep, remote access and cooling on the same machine.

| 순서 | 한국어 | English |
|---|---|---|
| ① 설치 / Installation | [인텔 T2 맥북에 Fedora KDE 설치하기: Ventoy USB와 EFI 부팅 복구](한국어/1.%20Fedora%20KDE%20설치와%20EFI%20부팅%20복구.md) | [Installing Fedora KDE on a T2 MacBook Pro: Ventoy USB and EFI boot recovery](English/1.%20Fedora%20KDE%20installation%20and%20EFI%20boot%20recovery.md) |
| ② 그래픽 / Display | [부팅은 됐는데 외부 화면은 왜 멎었을까?](한국어/2.%20Radeon%20외부%20화면%20멈춤.md) | [Fedora booted. Why did the external display still stall?](English/2.%20Radeon%20external%20display%20stalls.md) |
| ③ 전원 / Power | [SSH로 작업 중인데 맥북은 왜 15분 만에 잠들었을까?](한국어/3.%20SSH%20작업%20중%20자동%20절전.md) | [Why did my Mac go to sleep during an SSH session?](English/3.%20Sleep%20during%20SSH%20sessions.md) |
| ④ 원격 화면 / Remote desktop | [원격 데스크톱에 연결했는데 잠금 배경만 보였다](한국어/4.%20KDE%20원격%20데스크톱%20잠금%20화면.md) | [A working remote desktop connection that showed only the lock screen](English/4.%20KDE%20remote%20desktop%20lock%20screen.md) |
| ⑤ 팬 / Cooling | [세 번째 팬이 없는데 왜 Fan3를 찾았을까?](한국어/5.%20두%20팬과%20Fan3%20오류.md) | [Why did Fedora ask for Fan3 on a two-fan MacBook Pro?](English/5.%20Fan3%20on%20a%20two-fan%20Mac.md) |

<h2 id="image-credits">사진과 그림 출처 · Image credits</h2>

1편의 사진은 Jack Baty가 찍은 [2019 16-inch MacBook Pro](https://commons.wikimedia.org/wiki/File:2019_16-inch_MacBook_Pro_%2849183242933%29.jpg)다. 같은 세대의 외형을 보여 주는 참고 사진이다. 축소하고 WebP로 변환한 사진에도 [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)을 적용한다. 0plan이 그린 설치 그림은 부팅 메뉴의 선택 순서이고, SSD 배치와 2~5편의 그림은 이 맥북에서 확인한 결과다.

Jack Baty’s photo shows the same MacBook Pro generation, not the machine tested here. The resized WebP retains the CC BY-SA 2.0 license. The diagrams are by 0plan: the boot-menu diagram shows the steps; the disk layout and the figures in parts 2–5 show observations from this Mac.

그림 속 제품 표식은 해당 제품을 가리킨다. Sources and terms for the product marks:

- **Fedora** — [공식 로고 · Fedora brand guidelines](https://docs.fedoraproject.org/en-US/project/brand/) · [상표 이용 조건 · Trademark guidelines](https://docs.fedoraproject.org/en-US/legal/trademarks/).
- **KDE** — [공식 클립아트 및 이용 조건 · KDE logo and usage terms](https://kde.org/stuff/clipart/). KDE는 로고 복제에 LGPL을 적용한다. KDE’s press kit permits using the logo to identify KDE.
- **Ventoy** — [프로젝트 공식 아이콘 · Ventoy’s project icon](https://www.ventoy.net/static/img/ventoy.png?v=1).
- **AMD Radeon** — [AMD의 로고 · AMD logo, via Wikimedia Commons](https://commons.wikimedia.org/wiki/File:AMD_Radeon_logo.svg). Commons는 이 글자 로고를 퍼블릭 도메인으로 분류한다. The Radeon trademark remains AMD’s.
- **Apache Guacamole** — [공식 로고 · Project logo](https://guacamole.apache.org/images/logos/guac-classic-logo.svg), © The Apache Software Foundation. [Apache License 2.0 및 저작권 고지 · License and notice](https://raw.githubusercontent.com/0plan/blog/main/assets/images/fedora-t2/guacamole-notices.txt).
