---
title: '테스트'
excerpt: '테스트 하는그다'
toc: true
toc_sticky: true
# header:
#    teaser: /assets/images/profile.png
categories:
   - Blog
# tags:
#    - Blog
last_modified_at: 2020-06-15T08:06:00-05:00
gallery2:
   - url: https://flic.kr/p/8a6Ven
     image_path: https://farm2.staticflickr.com/1272/4697500467_8294dac099_q.jpg
     alt: 'Black and grays with a hint of green'
   - url: https://flic.kr/p/8a738X
     image_path: https://farm5.staticflickr.com/4029/4697523701_249e93ba23_q.jpg
     alt: 'Made for open text placement'
   - url: https://flic.kr/p/8a6VXP
     image_path: https://farm5.staticflickr.com/4046/4697502929_72c612c636_q.jpg
     alt: 'Fog in the trees'
---

✓ ✓ ☑ ❤ ⁂ ☽ ☹ ☺ ☻ ✈ ✉  
→ ← ↑ ↓ ↔ ↗ ↙ ↖ ↘  ↗
✁ ✂ ✄ ✎ ✏ ✐  
✓ ✔ ✕ ✖ ✗ ✘
⚑ ⚐ 
⬆➡⬇⬅

<strike>strikeout text</strike>

이미지는 절대경로  
![Alt text](/assets/images/profile.png)

<address>
  1 Infinite Loop<br /> Cupertino, CA 95014<br /> United States
</address>
This is an example of a [link](http://apple.com'Apple').

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice}` class.
{: .notice}

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice--primary}` class.
{: .notice--primary}

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice--info}` class.
{: .notice--info}

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice--warning}` class.
{: .notice--warning}

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice--success}` class.
{: .notice--success}

**Watch out!** This paragraph of text has been [emphasized](#) with the `{: .notice--danger}` class.
{: .notice--danger}

만약에 이미지가 엄청 큰 메인으로 되는 게시글을 쓰고 싶다면

```md
---
title: 'Layout: Header Image and Text Readability'  # 이건 제목
header:
   image: /assets/images/unsplash-image-4.jpg  #경로추가
   caption: 'Photo credit: [**Unsplash**](https://unsplash.com)'   #아래쪽캡션
<!-- tags:
   - sample post
   - readability
   - test -->
   categories:
   - Blog
---
```
갤러리? 형식같은경우 위에 변수처럼 선언해두고 가져다가 쓰면된다.
{% include gallery id="gallery2" caption="This is a second gallery example with images hosted externally." %}

[Default Button](#){: .btn}
[Primary Button](#){: .btn .btn--primary}
[Success Button](#){: .btn .btn--success}
[Warning Button](#){: .btn .btn--warning}
[Danger Button](#){: .btn .btn--danger}
[Info Button](#){: .btn .btn--info}
[Inverse Button](#){: .btn .btn--inverse}
[Light Outline Button](#){: .btn .btn--light-outline}



[X-Large Button](#){: .btn .btn--primary .btn--x-large}
[Large Button](#){: .btn .btn--primary .btn--large}
[Default Button](#){: .btn .btn--primary }
[Small Button](#){: .btn .btn--primary .btn--small}
