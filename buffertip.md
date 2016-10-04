# VIM buffer 를 IDE 탭처럼 사용하기

* [버퍼와 윈도우, 탭을 용도에 따라 적절히 섞어 사용하기](#버퍼와-윈도우-탭을-용도에-따라-적절히-섞어-사용하기)
* [버퍼 위주로 사용하기](#버퍼-위주로-사용하기)
    * [버퍼 기본 사용법](#버퍼-기본-사용법)
    * [airline 으로 더 편하게 사용하기](#airline-으로-더-편하게-사용하기)
    * [map 설정으로 더 편하게 사용하기](#map-설정으로-더-편하게-사용하기)
* [탭 위주로 사용하기](#-탭-위주로-사용하기)
    * [탭 기본 사용법](#탭-기본-사용법)
    * [탭 위주 사용의 문제점](#탭-위주-사용의-문제점)
* [파일 검색 플러그인 추천](#파일-검색-플러그인-추천)

------

* 이 문서에서는 IDE에 익숙한 VIM 초심자를 위해 버퍼와 윈도우와 탭을 IDE 처럼 관리하는 노하우를 소개합니다.
* Visual Studio, Eclipse, Atom, IntelliJ IDEA 같은 현대적인 IDE에서는 buffer, window, tab을 모두 고려할 필요가 없습니다. 
IDE 세상에서 탭은 곧 파일이며, 탭을 닫으면 파일도 함께 닫힙니다.
즉 버퍼와 윈도우개념은 탭으로 통일되었고, 사용자는 탭을 전환하며 각 파일을 편집합니다.
* 메모리 상의 파일과 탭의 일치는 굉장히 심플하고 강력합니다. 하지만 VIM은 8 버전이 나온 2016년 현재까지도 버퍼와 윈도우와 탭을 따로 관리할 수 있는 기능을 제공하며 이는 IDE에 비해 불편하긴 하지만 사용자에게 더 많은 자유를 줍니다.

## 버퍼와 윈도우, 탭을 용도에 따라 적절히 섞어 사용하기
* 가장 이상적인 방법이지만 VIM의 구조와 다양한 명령어를 알고 있어야 가능하기 때문에 초심자에게는 쉽지 않습니다. 다음 절의 [버퍼 위주로 사용하기](#버퍼-위주로-사용하기) 방법을 추천합니다.

## 버퍼 위주로 사용하기

#### 버퍼 기본 사용법
탭은 하나만 사용하면서 버퍼를 전환해가며 파일을 편집하는 방법이라 할 수 있습니다.

* 편집할 파일을 열 때에는 `:e filename`을 씁니다.
* 편집중인 파일을 닫을 때에는 `:bd!`를 씁니다.
* 오른쪽에 새로운 윈도우를 열기 위해서는 `:vs`를 씁니다.
* 아래쪽에 새로운 윈도우를 열기 위해서는 `:sp`를 씁니다.
* 버퍼 목록을 확인하려면 `:ls`를 씁니다. 여기에서 버퍼 번호를 볼 수 있습니다.
* 버퍼 이동을 하려면 `:b 버퍼번호` 를 씁니다.

#### airline 으로 더 편하게 사용하기
버퍼 목록을 IDE의 탭처럼 보고 싶다면 [vim-airline](https://github.com/vim-airline/vim-airline) 플러그인을 설치한 다음, 다음과 같이 설정해주면 됩니다.

```viml
let g:airline#extensions#tabline#enabled = 1              " vim-airline 버퍼 목록 켜기
let g:airline#extensions#tabline#fnamemod = ':t'          " vim-airline 버퍼 목록 파일명만 출력
let g:airline#extensions#tabline#buffer_nr_show = 1       " buffer number를 보여준다
let g:airline#extensions#tabline#buffer_nr_format = '%s:' " buffer number format
```

#### map 설정으로 더 편하게 사용하기
그리고 다음과 같이 버퍼 이동 map 도 추가해 줍시다.

```viml
nnoremap <C-S-t> :enew<Enter>         " 새로운 버퍼를 연다
nnoremap <C-F5> :bprevious!<Enter>    " 이전 버퍼로 이동
nnoremap <C-F6> :bnext!<Enter>        " 다음 버퍼로 이동
nnoremap <C-F4> :bp <BAR> bd #<Enter> " 현재 버퍼를 닫고 이전 버퍼로 이동
```
위와 같이 설정하면 IDE의 탭과 비슷한 느낌으로 버퍼를 사용할 수 있습니다.

## 탭 위주로 사용하기

#### 탭 기본 사용법
버퍼 하나당 탭 하나씩을 사용하는 방법입니다. IDE와 거의 똑같은 느낌으로 파일을 관리하며 편집할 수 있습니다.

* 편집할 파일을 열 때에는 `:tabe filename`을 씁니다.
* 편집중인 파일을 닫을 때에는 `:q`를 씁니다.
* 오른쪽에 새로운 윈도우를 열기 위해서는 `:vs`를 씁니다.
* 아래쪽에 새로운 윈도우를 열기 위해서는 `:sp`를 씁니다.
* 탭 목록을 확인하려면 화면 위쪽을 보면 됩니다. 탭 목록이 나와 있기 때문입니다. 물론 `:tabs`를 입력해도 됩니다.
* 탭 이동을 하려면 `gt` 와 `gT`를 씁니다.

#### 탭 위주 사용의 문제점
버퍼 하나당 탭을 하나씩 열어 사용하는 방법은 VIM을 자연스럽게 사용하는 방법과는 거리가 있다고 할 수 있습니다.
* 보통 IDE라면 탭을 닫을 때 파일도 닫히지만, VIM에서는 탭을 닫아도 버퍼는 남아 있습니다. 스크린에서만 사라진 것입니다.
* 따라서 여러 파일을 편집하며 탭만을 열고 닫고 하다 보면, 편집한 많은 파일들이 메모리 상에 그대로 남아 있게 됩니다.
* 탭은 윈도우의 집합을 정의하기 위한 것이기 때문에 하나의 파일에 하나의 탭을 여는 것은 탭 개념의 낭비입니다.

따라서 탭 위주로 사용하기로 마음먹었다면, 해당 탭을 닫을 때 버퍼도 확실히 삭제해 주는 편이 좋습니다.

## 파일 검색 플러그인 추천
IDE를 사용하다가 VIM을 사용할 때의 불편한 점들 중 가장 치명적인 단점은
다른 파일을 버퍼로 불러오고 싶을 때 `:e` 명령으로 파일을 검색하는 데에 한계가 있다는 것입니다.
모든 파일이 한 곳에 모여 있는 경우는 매우 드물기 때문에
VIM으로 여러 파일을 편집하는 건 굉장히 골치아픈 일일 수 있습니다.

이런 불편을 해소하기 위해서는 별도의 플러그인을 설치하는 것이 편합니다.
다음 플러그인들을 추천합니다.

* [fzf.vim](https://github.com/junegunn/fzf.vim)
    * 장점 : 엄청나게 빠릅니다.
    * 단점 : gui VIM 에서 사용하기 위해서는 추가 설정이 필요합니다. 설정을 해도 검색할 때마다 추가 터미널이 나타나기 때문에 불편한 점이 좀 있습니다.
* [ctrlp.vim](https://github.com/kien/ctrlp.vim)
    * 장점 : fzf 와는 달리 gui vim 에서도 별다른 문제가 없습니다.
    * 단점 : 딱히 없습니다.

두 플러그인을 모두 사용해 보고 자신에게 맞는 것을 선택해 쓰시면 됩니다.

개인적으로는 주로 터미널 VIM을 사용하기에 fzf가 매우 만족스럽습니다.
> MacVIM도 -v 옵션을 주면 gui가 나타나지 않고 터미널에서 사용할 수 있으므로 alias를 지정해 사용하는 것도 좋은 방법입니다.
