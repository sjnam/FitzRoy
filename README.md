mpresty
==================
An openresty web application for TeX graphics

Getting started
---------------
You can write the `metapost` or `tikz` or `graphviz` script of the image you want to
draw in the html file, and the _mpresty_ converts the script into the image.

The graphics scripts are inserted into the html in codes or they are stored in
a file and inserted into uri format of `img`'`src` attribute.

````html
<html>
<body>

<H1>Metapost</H1>
<metapost width="200">
beginfig(1)
  u:=1.3cm; transform T; z1=(0,2u); n:=5;
  for i=1 upto n-1: z[i+1]=z1 rotated (360*i/n);
  endfor;
  z1 transformed T=0.1[z1,z2];
  z2 transformed T=0.1[z2,z3];
  z3 transformed T=0.1[z3,z4];
  path p;
  p = for i=1 upto n: z[i]--endfor cycle;
  for i=0 upto 100:
    fill p withcolor 0.2*white; p:=p transformed T;
    fill p withcolor white;     p:=p transformed T;
  endfor;
endfig;
</metapost>
<img src="/mpresty/tree.mp" width="200" cache="no">
<img src="http://ktug.org/~sjnam/source/rgb.mp" width="200">

<H1>Graphviz</H1>

<graphviz cmd="dot" width="200">
digraph G {
    main -> parse -> execute;
    main -> init;
    main -> cleanup;
    execute -> make_string;
    execute -> printf
    init -> make_string;
    main -> printf;
    execute -> compare;
}
</graphviz>
<img src="https://graphviz.org/Gallery/directed/Linux_kernel_diagram.gv.txt" cmd="dot" width="200">
<img src="http://ktug.org/~sjnam/source/neato.gv" width="200" cmd="neato">

<H1>tikz</H1>

<tikz width="200">
\begin{tikzpicture}[scale=3]
  \draw[step=.5cm, gray, very thin] (-1.2,-1.2) grid (1.2,1.2); 
  \filldraw[fill=green!20,draw=green!50!black] (0,0) -- (3mm,0mm) arc (0:30:3mm) -- cycle; 
  \draw[->] (-1.25,0) -- (1.25,0) coordinate (x axis);
  \draw[->] (0,-1.25) -- (0,1.25) coordinate (y axis);
  \draw (0,0) circle (1cm);
  \draw[very thick,red] (30:1cm) -- node[left,fill=white] {$\sin \alpha$} (30:1cm |- x axis);
  \draw[very thick,blue] (30:1cm |- x axis) -- node[below=2pt,fill=white] {$\cos \alpha$} (0,0);
  \draw (0,0) -- (30:1cm);
  \foreach \x/\xtext in {-1, -0.5/-\frac{1}{2}, 1} 
    \draw (\x cm,1pt) -- (\x cm,-1pt) node[anchor=north,fill=white] {$\xtext$};
  \foreach \y/\ytext in {-1, -0.5/-\frac{1}{2}, 0.5/\frac{1}{2}, 1} 
    \draw (1pt,\y cm) -- (-1pt,\y cm) node[anchor=east,fill=white] {$\ytext$};
\end{tikzpicture}
</tikz>
<img src="http://ktug.org/~sjnam/source/bissector.tex" width="200">
<img src="http://ktug.org/~sjnam/source/mosaic.tex" width="200">

</body>
</html>
````

Run
---
```bash
% git clone https://github.com/sjnam/mpresty.git
% cd mpresty
% mkdir -p html/images
% docker run -d -p 8080:8080 \
-v $(pwd)/lua:/usr/local/openresty/nginx/lua \
-v $(pwd)/html:/usr/local/openresty/nginx/html \
-v $(pwd)/conf.d:/etc/nginx/conf.d \
--name mpresty sjnam/mpresty
```

Try to visit the following pages
- http://localhost:8080/mpresty/tutorial.html
([Original html page](http://www.ursoswald.ch/metapost/tutorial.html))
- http://localhost:8080/mpresty/sunflower.html
- http://localhost:8080/mpresty/all.html
- http://localhost:8080/preview.html

Create a `fun.html` file with the above [Getting started](#getting-started) and
put it in the `$(pwd)/html` directory and visit http://localhost:8080/fun.html

Advanced Usage
--------------
### update-node customization
See [upnode.lua](https://github.com/sjnam/mpresty/blob/master/lua/upnode.lua).
- http://localhost:8080/upnode/all.html

Author
------
Soojin Nam, jsunam at gmail.com
