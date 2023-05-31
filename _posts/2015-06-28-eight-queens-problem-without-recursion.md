---
id: 14
title: 'Eight Queens Problem without Recursion'
date: '2015-06-28T23:03:12+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=14'
permalink: /2015/06/eight-queens-problem-without-recursion/
categories:
    - 'Algorithm &amp; Data Structure'
    - Programming
---

#### <span style="text-decoration: underline;"><span style="color: #0000ff;">[Source File Download](http://pittnuts.com/pub/code/eightqueen.cpp)</span></span> for Eight Queens Problem without Recursion

```
<pre class="EnlighterJSRAW" data-enlighter-language="cpp">//
//  main.cpp
//  eightqueen
//
//  Created by pittnuts on 6/28/15.
//  Copyright (c) 2015 Wesley. All rights reserved.
//

#include <iostream>

int main()
{
    int here[8] = {-1,-1,-1,-1,-1,-1,-1,-1};//visited positions for i-th row
    int a[8]; //A queen on i-th column
    int b[15];//A queen on the “/” diagonal
    int c[15];//A queen on the “\” diagonal
    int count=0;
    memset(a,0,sizeof(int)*8);
    memset(b,0,sizeof(int)*15);
    memset(c,0,sizeof(int)*15);
    
    int line = 0;
    bool matched;
    while( line>=0 ){//check if back tracking finished
        matched = false;
        for (int i =here[line]+1;i<8;i++){//search begins at the position previously visited
            if (0==a[i] && 0==b[line+7-i] && 0==c[line+i]){//the first matched position
                if (here[line]>=0){//clear original record
                    a[here[line]] = b[line+7-here[line]] = c[line+here[line]] = 0;
                }
                here[line] = i;
                a[i] = b[line+7-i] = c[line+i] = 1;
                matched = true;
                break;
            }
        }
        
        if (matched){
            line++;//next row
            if (line == 8){
                count++;
                line--;
            }
        }else{
            if (here[line]>=0){//clear original record
                int tmp = here[line];
                here[line] = -1;
                a[tmp] = b[line+7-tmp] = c[line+tmp] = 0;
            }
            line--;//back tracking
        }
    }
    printf("%d\n",count);//print total (92) solutions
    return 0;
}
```