# LZW Compression

Python(3) based implementation of a utf-8 encoded text file compression using the [Lempel-Ziv-Welch](https://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Welch) text file compression algorithm. The primary purpose is to tweak various parameters and see their effect on compression ratio and time.


# Installing

These instructions will get you a copy of the project up and running on your local machine for development.

Firstly, the lzw package needs to be installed. I highly recommend using a virtual environment to install this package as it is a distutils install and not a pip one.
For information on virtual environments see [this](https://virtualenv.pypa.io/).

**Note**: Make sure you are using python3 and not python2. The version of python3 I used to build this project is python3.5.2 and I do not guarantee any backward/forward compatibility.

Open a new terminal window(recommended).

After cloning the repository or downloading the the source distribution tarball and unzipping, cd into the folder of the project and run the following command in the terminal:

```
python3 setup.py install
```

This completes the installation of the package. Now its time to use this package to achieve some compression.


# Getting started

There are **2 major classes** in the lzw package that form the functional API. First one is **compress** that resides in the **lzw.Compress** module and another is **decompress** which is defined in **lzw.Decompress** module. The basic steps in compression and decompression involve simply instantiating these classes with appropriate file names and paths and then calling the encode and decode methods on these objects respectively. Following sections provide a walkthrough of these steps in detail.

## Compression
First import the compress class as follows:

```
from lzw.Compress import compress as comp
```

Then create a compress object as follows:

```
c = comp('/path/to/input/file','/desired/path/for/output/file'[[[[,limit,encoding,max_utf_char,verbose,chunks]]]])
```

**Note**: The input file must be in the utf-8 encoding format. Provide entire path as a string input to the class. Provide the path where the compressed file is to reside. Do not provide a filename as the output path. The name of the compressed file will default to a "\_compressed.txt" appended to your input file's name.

**limit**: (default=20000000,type=int) is an optional argument which specifies the max size of the file to be compressed. The default is 20MB. The limit can be changed but note that larger files take substantially large times(as of now).

**encoding**: (type=str,default=ascii_255) This is arguably the most important argument so far the
compression efficiency and time are concerned. There are 3 options for this argument: 'ascii_255',
ascii_127' and 'utf-8'. ascii_127 and ascii_255 are chosen when the file contains characters within the first 256 and 128 utf-8(or ascii) charset respectively. 'utf-8' is a generic option and requires the max_utf_char(int) value to be specified. The algorithm then uses an initial dictionary of characters upto and including the max_utf_char character.

**max_utf_char**: (type=int,default=None) Only applicable(and required) if encoding='utf-8'. You will achieve maximum compression in minimum time if max_utf_char is set to not more than the maximum utf-8 value among all characters present in your file. Maximum value for this argument = os.maxunicode.

**verbose**: (default=0,type=int) input, if set to 2, the program displays percent execution per chunk of input processed.
verbose=1 shows the execution time per chunk of input file processed.
Note: The verbose=2 option may generate large amounts of output on stdout and is 0
by default.

**chunks**: (default=None,type=int) is an integer type argument used to specify the number of chunks in which the file is divided in during compression. By default, the program adoptively decides this number. chunks are useful for very large files as a single chunk is loaded into RAM during compression. The maximum value allowed is 100 chunks.

After the command runs successfully, the compress object is ready. The actual compresseion is triggered by calling the **encode** method on this object as follows:

```
c.encode()
```

Depending on the file size, this may take quite a while. The status is shown in the terminal window.
Once done, a new file is created in the path you mentioned which you can verify is decently compressed to about a third of its original size provided the original file was not trivially small.

## Decompression

The steps here are similar to that of compression.

Firstly, import the decompress class as follows:

```
from lzw.Decompress import decompress as dec
````

Next, create a decompress object as follows:

```
d = dec('/path/to/compressed/file','/desired/path/for/decompressed/file'[[[[,limit,encoding,max_utf_char,verbose,chunks]]]])
```

**Note**: The first argument is the full path of the file to be decompressed. This file must have a .txt extension and must be a one generated by the compress class of this package. The desired path for the decompressed file is the second argument. Do not provide a file name here and give a full qualified path. The name of the decompressed file will default to a "\_decompressed.txt" appended to your input file's name.

**limit**: is an optional(integer) argument which specifies the max size of the file to be decompressed. The default is 20MB. The limit can be changed but note that larger files take substantially large times(as of now).

**encoding**:(type=str,default=ascii_255) The value here must be the same as that used while compressing the file.

**max_utf_char**: (type=int,default=None) This parameter is required iff encoding='utf-8' and must be the same as that used while compression.

**verbose**(type=int,default=0) is similar to the argument to lzw.Compress.compress.

**chunks**: (default=None,type=int) is an integer type argument used to specify the number of chunks in which the file is divided in during decompression. By default, the program adoptively decides this number. chunks are useful for very large files as a single chunk is loaded into RAM during decompression. The maximum value allowed is 100 chunks.

After the command runs successfully, the decompress object is ready. The actual decompression is triggered by calling the *decode* method on this object as follows:

```
d.decode()
```

Depending on the file size, this may take quite a while. The status is shown in the terminal window.
Once done, a new file is created in the path you mentioned which must be the same as the input file.

**Note**: The default extension for the decompressed file is *.txt*. You can suitably rename it to your desired format.
For maximum compression and minimal execution time, use the encoding='utf-8' option and provide the max_utf_char argument equal to the maximum of the utf-8(int) values of all characters in the file.
This gives both maximum compression in minimum time as the initial dictionary is small in size.
In case you do not know the characters that will be present in the file, assume a range and choose the max_utf_char value in its ballpark. If a character which has its utf-8 value greater than the one you mentioned, an error shall be raised during execution.
Note that the LZW algorithm iterates over your file just(and exactly) once while compression.
Also, remember to use the same set of arguments for the decompression object for faithful results.


# Author

 **Prathamesh Mandke** - mandkepk97@gmail.com

# License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
