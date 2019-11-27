# How to Intall metis 5.1.0 in Win10

最近研究一篇論文  
2019 Cluster-GCN: An Efficient Algorithm for Training Deep and Large Graph Convolutional Networks  
論文實現需要使用graph clustering 的工具 [Metis](http://glaros.dtc.umn.edu/gkhome/metis/metis/download)
除此之外 還需要python的[warper](https://metis.readthedocs.io/en/latest/#installation) (這個安裝用 pip install即可，不多說明)  
在安裝的過程中花了很多時間採坑，中間看到有個pymetis的套件看起來安裝好像比較簡單，但是遇到一堆問題  
最後還是按照paper的做法安裝metis 與python warpper，因此記錄安裝的流程並分享給大家

## Envirement

OS: Win10 x64  
Python 3.7

## Requirement

1. [Cmake 2.8](https://cmake.org/) (in metis document)
2. [Visual Studio](https://visualstudio.microsoft.com/zh-hant/downloads/?rr=https%3A%2F%2Fwww.google.com%2F)
 
這裡我選擇安裝  Cmake 3.16  與 Visual Studio 2019(x64) 
順便一提，會選擇這樣的版本是因為使用 Cmake 建立 metis 的時候，Cmake 2.8 只支援到 Visual Studio 2013  
相信只要抓相容的Visual Studio，Cmake 2.8 也是可以的  
Visual Studio 安裝的時候記得勾選 "使用C++的桌面開發"(desktop application development using c++)


```python
%%html
<img src="vs2019.png">
```


<img src="vs2019.png">



## Download metis and unzip

你可以在這裡下載[Metis](http://glaros.dtc.umn.edu/gkhome/metis/metis/download)  
下載完畢後解壓縮

## Metis document

將Metis 打開以後，你可以在裡面看到三個安裝文檔: "install.txt" , "Build.txt" , "Build-Windows.txt"

### Install.txt


These are some preliminary instructions for the 5.0 release of METIS.

1. You need to have a C compiler that supports the C99 standard. 
   Gcc works just fine, but I have not tested it on many other architectures
   (any feedback/patches for different architectures are welcomed)
   
2. You need to have GNU make and CMake 2.8 (http://www.cmake.org/) installed.

3. Edit the file include/metis.h and specify the width (32 or 64 bits) of the
   elementary data type used in METIS. This is controled by the IDXTYPEWIDTH
   constant.

   For now, on a 32 bit architecture you can only specify a width of 32, 
   whereas for a 64 bit architecture you can specify a width of either 
   32 or 64 bits.


4. At the top of Metis' directory execute 'make' and follow the instructions.
   
      make

5. To build on windows using Visual Studio follow the instructions in the
   file BUILD-Windows.txt.


### Build-Windows.txt

Building METIS requires CMake 2.8, found at
http://www.cmake.org/. CMake generates Visual Studio project files,
which then can be built using Visual Studio. There are two ways to
generate visual studio files: using the command line and using the
CMake GUI.

<font size=4>Using the command line</font>

Open the command prompt and cd to the METIS source directory. Run

    > cmake --help

and look at the list of generators for the Visual Studio studio you
want to build for. For example, the generator for Visual Studio 2010
is called "Visual Studio 10".

After you have found the appropriate generator, run

    > .\vsgen -G "<GENERATOR>"

to generate the project files. The project files will be placed
build\windows.


<font size=4>Using the CMake GUI</font>

It is also possible to use the CMake GUI, distributed with CMake. To
do this, open the CMake GUI, and browse to the location of METIS'
source with the "Browse Source" button. You can also change the binary
directory. This is where the Visual Studio project files will be
placed. Click "Generate" to select the correct visual studio version
and build the project files.

<font size=4>Using the VS project files</font>

The Visual Studio project will be called METIS.sln. Open it in Visual
Studio. If the configuration is not already "Release", set it to
"Release". Type F7 to build. The METIS library will be in
<BINARY_DIR>\libmetis\Release and the executable programs will be in
<BINARY_DIR>\programs\Release. (<BINARY_DIR> will be build\windows if
you used the command line or whatever you choose if using the CMake
GUI.)


##  安裝步驟Install Step: 
1. (X64 )編輯 metis-5.1.0\include\metis.h 裡面的 IDXTYPEWIDTH 修改為64
  
2. 開啟 command line，並到metis路徑中執行:
    >cmake--help  

   這時候會跑出一堆generator 的list，如 "Visual Studio 16 2019"  
   如果沒有，就代表你的Cmake 版本跟 VS 不匹配，要再去找相容的版本  
     
3. 確認沒問題後，執行:  
   > .\vsgen -G "Visual Studio 16 2019"  
   
   會跑出 <font color=red>Cmake Error: Couldn't create named Visual Studio 16 2019</font>
4. 研究了一下，將環境變數的路徑中Cmake 的路徑順訊提前可以解決這個問題
5. 回到step 3. ，重新執行指令，會發現沒有報錯
   但是metis-5.1.0/ build/ windows /裡面仍然是空的
6. 將指令修改為:  
   > <font color=red>.\vsgen -G "Visual Studio 16 2019" -A x64</font>  

   就會正常建立了
7. 用VS 把 metis-5.1.0/build/windowns/METIS.sln 打開，我安裝的結果方案組態(configuration)就已經是Release的狀態了


```python

```
