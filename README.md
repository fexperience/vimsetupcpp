
# Setup C++ gvim
## 1. Cài Vim/GVim

- **Windows**: tải GVim tại [đây](https://www.vim.org/download.php)  
- Cần **MinGW** và đã path biến
- Cài đặt [ Font Nerd](https://www.nerdfonts.com/font-downloads)

## 2. Tạo tệp cấu hình

- Mở tệp ``` _vimrc``` ở ```C:\Users\<Your Name>```
- Dán đoạn code này vào ```_vimrc```
```vim
source $VIMRUNTIME/vimrc_example.vim
au GUIEnter * simalt ~x
set hls
set is
set cb=unnamed
set guifont=0xProto\ Nerd\ Font\ Mono:h20
set ts=4
set sw=4
set si
nnoremap <F2> :NERDTreeToggle F:\\<CR>
inoremap { {}<Left>
inoremap {<CR> {<CR>}<Esc>O
inoremap {{ {
inoremap {} {}
autocmd BufNewFile *.cpp 0r ~/.vim/templates/temp.cpp
autocmd filetype cpp nnoremap <F9> :w <bar> !g++ -std=c++14 % -o %:r -Wl,--stack,268435456<CR>
autocmd filetype cpp nnoremap <F10> :!%:r<CR>
autocmd FileType cpp nnoremap <F11> :w <bar> !g++ -std=c++14 % -o %:r -Wl,--stack,268435456 && %:r<CR>
autocmd filetype cpp nnoremap <C-C> :s/^\(\s*\)/\1\/\/<CR> :s/^\(\s*\)\/\/\/\//\1<CR> $
set nu
augroup numbertoggle
    autocmd!
    autocmd BufEnter,FocusGained,InsertLeave * set rnu
    autocmd BufLeave,FocusLost,InsertEnter * set nornu
augroup END
set diffexpr=MyDiff()
function MyDiff()
  let opt = '-a --binary '
  if &diffopt =~ 'icase' | let opt = opt . '-i ' | endif
  if &diffopt =~ 'iwhite' | let opt = opt . '-b ' | endif
  let arg1 = v:fname_in
  if arg1 =~ ' ' | let arg1 = '"' . arg1 . '"' | endif
  let arg1 = substitute(arg1, '!', '\!', 'g')
  let arg2 = v:fname_new
  if arg2 =~ ' ' | let arg2 = '"' . arg2 . '"' | endif
  let arg2 = substitute(arg2, '!', '\!', 'g')
  let arg3 = v:fname_out
  if arg3 =~ ' ' | let arg3 = '"' . arg3 . '"' | endif
  let arg3 = substitute(arg3, '!', '\!', 'g')
  if $VIMRUNTIME =~ ' '
    if &sh =~ '\<cmd'
      if empty(&shellxquote)
        let l:shxq_sav = ''
        set shellxquote&
      endif
      let cmd = '"' . $VIMRUNTIME . '\diff"'
    else
      let cmd = substitute($VIMRUNTIME, ' ', '" ', '') . '\diff"'
    endif
  else
    let cmd = $VIMRUNTIME . '\diff'
  endif
  let cmd = substitute(cmd, '!', '\!', 'g')
  silent execute '!' . cmd . ' ' . opt . arg1 . ' ' . arg2 . ' > ' . arg3
  if exists('l:shxq_sav')
    let &shellxquote=l:shxq_sav
  endif
endfunction
syntax on
filetype plugin indent on
augroup cpp_syntax_additions
  autocmd!
  autocmd FileType cpp syntax keyword cppKeyword noexcept nullptr static_assert constexpr decltype override final thread_local alignas alignof char8_t char16_t char32_t co_await co_yield co_return concept requires import module
  autocmd FileType cpp syntax keyword cppKeyword cin cout cerr
  autocmd FileType cpp syntax keyword cppSTLAlgorithm sort stable_sort partial_sort nth_element lower_bound upper_bound binary_search equal_range
  autocmd FileType cpp syntax keyword cppSTLAlgorithm max min max_element min_element swap reverse rotate next_permutation prev_permutation 
  autocmd FileType cpp syntax keyword cppSTLAlgorithm push_back pop_back emplace_back insert erase clear size empty begin end count find 
  autocmd FileType cpp syntax keyword cppSTLAlgorithm fill copy transform unique remove to_string stoll

  autocmd FileType cpp syntax keyword cppSTLContainer map multimap unordered_map set multiset unordered_set vector deque queue stack priority_queue pair make_pair string
augroup END
augroup cpp_bracket_highlight
  autocmd!
  autocmd FileType cpp syntax match cppParen /[\(\)\[\]\{\}]/
augroup END
highlight cppKeyword ctermfg=28 guifg=#006400
highlight cppSTLAlgorithm ctermfg=130 guifg=#996633
highlight cppSTLContainer ctermfg=130 guifg=#996633
highlight cppParen ctermfg=Magenta guifg=MediumOrchid
call plug#begin('~/.vim/plugged')
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'preservim/nerdtree'
Plug 'plasticboy/vim-markdown'
Plug 'SirVer/ultisnips'
Plug 'honza/vim-snippets'
Plug 'morhetz/gruvbox'
Plug 'sainnhe/everforest'
Plug 'folke/tokyonight.nvim', { 'branch': 'main' }
Plug 'octol/vim-cpp-enhanced-highlight'
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
Plug 'ryanoasis/vim-devicons'
Plug 'dracula/vim'
call plug#end()

set laststatus=2
set background=dark
let g:dracula_background = 'hard'
let g:dracula_enable_italic = 1
let g:dracula_disable_italic_comment = 0
syntax enable
colorscheme dracula
let g:airline_theme = 'dracula'
let g:airline_section_a = '%f %{airline#extensions#filetype#get()}'
let g:airline#extensions#branch#enabled = 1
let g:airline#extensions#branch#show_count = 1
let g:airline_section_a = '%f' 
let g:airline_section_c = '%=  %l/%L  %p%%' 
let g:airline#extensions#quickfix#enabled = 1
let g:airline#extensions#lsp#enabled = 1
let g:airline_section_y = '%{strftime("%H:%M")}'  
let g:airline_section_z = '%p%%'
function! NERDTreeStatus()
  if exists('t:NERDTree')
    return '[NERDTree]'
  else
    return ''
  endif
endfunction
let g:airline_section_x = '%{NERDTreeStatus()}'
let NERDTreeWinSize=15
```

- Vào gvim chạy lệnh sau
```:PlugInstall```

---

## 3. Sử dụng

- **Toggle NERDTree**: `<F2>`  
- **Biên dịch C++**: `<F9>`  
- **Chạy chương trình**: `<F10>`
- **Biên dịch và chạy**: `<F11>`  
- **Tự động chèn template**: tạo file `.cpp` mới

---

## 4. Tinh chỉnh

- Thay đổi phím tắt: sửa `nnoremap <F2>`  
- Thay đổi template c++ : chỉnh file `~/.vim/templates/temp.cpp`  
- Thay đổi theme/font: sửa `colorscheme dracula` hoặc `set guifont`

