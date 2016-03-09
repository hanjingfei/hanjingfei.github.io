---
layout: post
title: "first_post"
date: 2016-03-07 14:08:32 +0800
comments: true
categories: 
---
## Code Highlight by Backtick Code Block

### verilog

``` verilog
always @(posedge hclk) begin : ID_MEM_EMPTY
    if(id_wpt != id_rpt)  id_mem_empty <= `TD 0;
    else id_mem_empty <=  `TD 1;
end
```

### tcl

``` tcl
foreach f [glob "*${file_name}*"] {
    puts [file normalize $f]
}
```

### vim

``` vim
function! GotoFileWithLineNum()
    " exclude comma from file name search
    " change the 'isfname' option temporarily to pick up file name in ncverilog MSG
    let bak_isfname = &isfname
    set isfname=@,48-57,/,.,-,_,+,#,$,%,~,="
    " filename under the cursor 
    let file_name = expand('<cfile>')
    if !strlen(file_name)
        echo 'NO FILE UNDER CURSOR'
        return
    endif

    " look for a line number separated by any :,"<space> 
    if search('\%#\f*[:, "]*\zs[0-9]\+')
        " change the 'iskeyword' option temporarily to pick up just numbers 
        let temp = &iskeyword
        set iskeyword=48-57
        let line_number = expand('<cword>')
        exe 'set iskeyword=' . temp
    endif

    exe 'set isfname=' . bak_isfname
    " edit the file 
    exe 'e '.file_name

    " if there is a line number, go to it 
    if exists('line_number')
        exe line_number
    endif
endfunction

map gf :call GotoFileWithLineNum()<CR>
map gsf :sp<CR>:call GotoFileWithLineNum()<CR>
```