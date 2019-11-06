---
layout: post
published: true
title: Hacking Together a Conan Package
date: '2019-11-06'
---
This is how I hacked together a Conan package for use with my [Windows console project](https://github.com/zethon/arcc). I wasn't concerned with having Conan automatically build the project's code, instead I wanted to compile the code myself and publish the resulting files so that my project could use them. 

I wrote about this [over a year ago](https://zethon.github.io/2018-10-16-conan/) when I first started using Conan, but my understanding then was even less than it is now (and it's still pretty minimal). I had to revisit this recently and I wanted to write up a more concise step-by-step guide on how I did this in case I have to do it again in another year.

1. Build the source code - This step should be obvious. In my case the results were some header files and a single \*.lib file.

2. Create a new folder for the package - In this case I created `c:\src\conan-pdcurses` since I wanted to package PDCurses.

3. Create a basic `conanfile.py` file - Luckily I still had the code from last year when I did this with [my other project](https://github.com/zethon/owl). The file ended up looking like this:

```
from conans import ConanFile, tools

class PDCursesConan(ConanFile):
    name = "pdcurses"
    version = "3.9"
    settings = "os", "compiler", "build_type", "arch"
    description = "<Description of Libtidy here>"
    url = "None"
    license = "None"

    def package(self):
        self.copy("*.h", dst="include", keep_path=False)
        self.copy("*.a", dst="lib", keep_path=False)
        self.copy("*.lib", dst="lib", keep_path=False)

    def package_info(self):
        self.cpp_info.libs = tools.collect_libs(self)
```
4. Create the package's folders - I created the following folders:<br/>
`c:\src\conan-pdcurses\package\lib`</br>
`c:\src\conan-pdcurses\package\include`

5. Copy the header and library files into the folder inside `package` - For PDCurses there were a couple of header files in multiple directories. I maintained that folder structure when I copied everything over.

6. `conan install .` - Run this command in the folder that you created in Step 2. This creates a few files that Conan uses.

7. `conan package . --package-folder=package` - This step may be redundant and/or unncessary but I ran it anyway. I have not tried this process without it.

8. `conan export-pkg . pdcurses/3.9@zethon/stable` - This creates the manifest file, which is necessary for uploading.

9.`conan upload pdcurses/3.9@zethon/stable -r arcc --all` - Finally we upload the package to the remote repository! In this case I had previously added `arcc` as a remote hosted on bintray.com.

## User Authentication

PDCurses was the first custom Conan package I needed for this project, so I created a new reposistory in Bintray called `arcc` (the name of the project). When I first ran Step 9 above I was met with this message:

```
Uploading pdcurses/3.9@zethon/stable to remote 'arcc'
Please log in to "arcc" to perform this action. Execute "conan user" command.
If you don't have an account sign up here: https://bintray.com/signup/oss
Remote 'arcc' username: zethon
Please enter a password for "zethon" account: <redacted>
```

My first thought was to use my bintray.com credentials, which did not work. Actually, the prompt that asked me to login didn't even work; instead it just hung whenever I entered my password. 

After a bit of searching, I discovered that I had to authenticate using an API Key. The API Key can be found in bintrary.com by hovering over your username and selecting `Edit Profile` and then selecting `API Key` on the left. Once I had the API key I was ready to authenticate the remote on my local machine:

`conan user -p <API KEY> -r arcc zethon`

Once I did that I was able to run Step 9 succesfully.

## Conclusion

After this was all done I was able to include PDCurses in my Conan settings as `pdcurses/3.9@zethon/stable`. 
