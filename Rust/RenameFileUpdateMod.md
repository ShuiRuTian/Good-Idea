how to importmod:
absolute mod
relative mod

special file name: 
"mod.rs"

mod places:
in file
modNameFile
modNameFolder + file

special situation:
mod ambiguity
mod shadowing(shadow ambiguity!)


Here is the mod tree:
crate
 └── front_of_house
     └── hosting

Here is the file tree:

src
 ├── lib.rs
 ├── front_of_house.rs
 └── front_of_house
     └── mod.rs

In mod.rs and front_of_house.rs, we have 
``` rs
pub mod hosting {
    pub fn add_to_waitlist() {}
}
```

Usually, in lib.rs, we have

```
mod front_of_house;                     // file for module `front_of_house` found at both front_of_house.rs and front_of_house\mod.rs delete or rename one of them to remove the ambiguity rustc(E0761)
use crate::front_of_house::hosting;     // rustc(E0432)
```

now, we change the file in 

but it is not the 

Q1: what about when there is ambiguity?
I think a resonable assumption is user want to rename the unwanted one to a new name. So ambiguity mod would not update.

Q2: what about change "mod.rs" file, from and to?



change file
file1        file2
file         /path1/file
/path1/file  file
/path1/file  /path2/file

change folder
folder1/*     folder2/*
folder1/*     folder1/folder2/*
folder1/folder2/*     folder1/*
