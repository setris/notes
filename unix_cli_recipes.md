### Extracting the contents of a tar.gz file
    
    tar -zxvf foo.tar.gz

or 

    tar -xf foo.tar.gz

**Explanation**: `foo.tar.gz` was created in the following way:
first, `tar` collected all the files into one package, `foo.tar`.
Then the `gzip` program applied compression to give us `foo.tar.gz`.
To reverse that process (decompress and extract), the command does
a couple of things:

* **f**: specifies the tar.gz file to be decompressed and extracted;
         this must be the last flag, followed by the name of the
         tar.gz file

* **z**: tells `tar` to decompress the archive using `gzip`
         (note: some implemetations of `tar`, such as `bsdtar`, recognize
         the compression algorithm automatically when reading archives;
         in that case, this flag is ignored. But it does not hurt to
         leave it in)

* **x**: extracts the files

* **v**: makes `tar`'s output verbose, showing all the files being
         extracted (optional)

One can optionally include the `-C` option to specify a directory to which the
contents should be extracted (must create directory first, though).

### Concatenate multiple files/directories into a single tar file

    tar -cvf foo.tar <multiple files and/or directories>

### Concatenate and compress multiple files/directories into a single tar.gz file

    tar -czvf foo.tar.gz <multiple files and/or directories>

### List contents of tar or tar.gz file

    tar -tf <foo.tar | foo.tar.gz>

### Recursively search file contents in a directory

    grep -r "string" .      # standard
    grep -ri "string" .     # ignore case when searching
    grep -r -l "string" .   # print only the filenames

    All the examples above deal with search the current working directory,
    but of course one can specify the search to start at any desired path.

### Recursively search file contents in a directory based on file extension

    grep --include=\*.{lux, java} -ri -e "foo" .

    In the above example, -e explicitly specifies the search string
