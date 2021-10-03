# Programming Algorithms in Lisp
[Programming Algorithms in Lisp](https://www.amazon.com/Programming-Algorithms-Lisp-Efficient-Programs/dp/1484264274)

심심할 때마다 읽고 예제 연습 합니다.

## Lisp으로 어떻게 알고리즘을 하지;;
![lisp](res/lisp.png)

이 내용은 교재의 섹션 3.5 "Getting Started"에서 소개하는 내용입니다.

### Rutils

```lisp
(ql:quickload :rutils)
(named-readtables:in-readtable rutils:rutils-readtable)
```

Rutils는 커먼 리습 매크로 활용도를 극한으로 높여 색다른 표현법을
제공하는 유틸리티 라이브러리입니다.

readtables는 rutils의 핵심 기능으로, '슬롯 바인딩'이라는 기능을
제공합니다. 이 기능은 `(elt (nth 1 (foo-slot 2 (bar-slot1 obj)) 0)`
같은 기다란 표현식을 `@obj.slot1.slot2#1#0` 처럼 간단하고 읽기 쉽게
표현하게 해 줍니다. 이외에도 다양한 함수와 매크로를 제공하여
알고리즘을 쉽게 이해할 수 있을 만큼 리습의 표현력을 크게 끌어올립니다.

### rlwrap

```
rlwrap sbcl
```

`rlwrap`은 줄단위 입력을 받지 않는 프로그램들을 더 쉽게 쓰기 위해
만들어졌습니다.  `rlwrap`을 이용하여 특수 제어문자를 입력하면
`^[[D`같은 못생긴 문자가 나오는 대신 커서가 이동합니다.

`sbcl`과 `ecl`은 `rlwrap`의 도움을 받아야 하는 대표적인 리습
구현체입니다. 그러나 `clisp`같은 경우에는 터미널 입력을 제대로
처리하게끔 구현되어 있어 `rlwrap`이 필요 없습니다.

교재에서는 `sbcl`을 사용한다고 밝혔습니다. 그러나 나는 `ecl`을 사용할
예정이고, 교재 내용을 따라가면서 구현체의 차이로 인한 문제가 생길 경우
메모를 적어둘 것입니다.

## LISP IDE

Lisp는 오랜 기간에 걸쳐 실제 애플리케이션을 만드는 데 쓰였기 때문에
분명 IDE 또한 존재합니다. 유명한 IDE로는 LispWork, Clozure CL이 있고,
에디터나 다른 범용 IDE의 플러그인 형태로 제공되는 것으론 Eclipse Cusp,
Emacs SLIME, Vim SLIMV, Portacle 등이 있습니다.

나는 여기서 교재에서 잠깐 언급된 Emacs SLIME을 사용할 것이고, 설정
방법을 남겨두겠습니다.

### SLIME 설정

이 내용을 `~/.emacs` 혹은 `~/.emacs.d/init.el`에 저장하고 재시작합니다.

리습 파일을 열고 `M-x slime`을 치면 슬라임 REPL이 로딩됩니다.

```elisp
;; 패키지 아카이브 초기화
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
(package-initialize)
;; use-package 설치
(when (not (package-installed-p 'use-package))
  (package-refresh-contents)
  (package-install 'use-package))

;; company: 자동완성 프론트엔드
;; flycheck: 문법 체크 프론트엔드
(use-package company
  :ensure t
  :config (global-company-mode 1))
(use-package flycheck
  :ensure t
  :config (global-flycheck-mode 1))

;; SLIME과 company 백엔드 설정
(use-package slime
  :ensure t
  :config
  ;; change this value to actual implemenation!
  (setq inferior-lisp-program "ecl")) 
(use-package slime-company
  :ensure t :after (slime company)
  :config
  (setq slime-company-completion 'fuzzy
        slime-company-after-completion 'slime-company-just-one-space))
(use-package slime-setup
  :no-require t
  :after (slime-company)
  (slime-setup '(slime-fancy slime-company slime-quicklisp slime-asdf)))
```
