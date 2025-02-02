# Keep In Mind
Though Telegram allows file uploads, it is not intended to be used as cloud storage. Your files could be lost at any time. Don't rely on this project (or any similar ones) for storing important files on Telegram.

# TelegramFS
A [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) program that stores files on Telegram.

Though I demonstrated Discord in the video too, I haven't included the code here. While I believe that storing your OWN files on Discord does NOT violate TOS, I think that spreading the code to do so might. Idk I'm trying to not actually get banned :)

## Usage and How to Run
### Requirements
- Linux. Sorry Windows gamers, probably WSL would work
- Python - I used 3.10.12
- libfuse3
- A non-negligible amount of memory, if you plan to upload large files. I ran this on a system
with 16GB RAM and that was usually enough, but I had occasional crashes uploading lots of large files in a row.


### To Run
Before running this, I recommend creating a virtual environment in Python.

- (optional) Create a venv with `python -m venv <your-env-name`
- Rename `.env.sample` to `.env` and fill in the variables with appropriate values. You can get the Telegram ones from  
- Run `pip install -r requirements.txt`.
    - This might fail to get pyfuse3. If it does, you may be missing some requirements. On my system (Ubuntu 20.04), running this command to install some exta packages worked for me:
    `sudo apt install meson cmake fuse3 libfuse3-dev libglib2.0-dev pkg-config`. You can also install from these directions: http://www.rath.org/pyfuse3-docs/install.html
- Run `python main.py <path/to/your/mount>` for instance, `python3 main.py ./telegramfs` will mount at the directory `telegramfs`` in the current working dir.

## Known Issues
Uploading large files to Telegram (more than ~3GB) may result in degraded performance or the system
killing the process for using too much memory. This is probably my fault. I found the behavior of memory management in Python is a bit strange, even calling gc.collect() after clearing the buffer doesn't always seem to work. It could also be an issue with the LRU cache I implemented... I don't have the patience to wait 20 minutes for it to crash and then debug, especially because the vast majority of my files are pretty small.

Error handling is somewhat lacking. If Telegram uploads fail, you'll probably see message in console, but it won't retry.

# TO UNMOUNT, IF SOMETHING BREAKS
`fusermount -u <path/of/your/mount>`
