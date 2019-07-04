# Setup to Ubuntu 18.04

 nginx, letsencrypt の設定は省く

reference:  
http://pynote.hatenablog.com/entry/python_ubuntu_pipenv  
https://qiita.com/hoto17296/items/845850c8854dd18f6fff



`$ sudo adduser --disabled-login tootcloud`

```
$ sudo apt install build-essential \
    curl \
    libbz2-dev \
    libffi-dev \
    liblzma-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    llvm \
    tk-dev \
    wget \
    xz-utils \
    zlib1g-dev \
    ca-certificates \
    git \
    python-dev \
    mecab
```

`$ sudo vi /etc/systemd/system/tootcloud.service`
```
    [Unit]
    Description = TootCloud uwsgi
    After=network.target
    
    [Service]
    User = tootcloud
    WorkingDirectory = /home/tootcloud/TootCloud
    Restart = always
    ExecStart = /home/tootcloud/.pyenv/shims/uwsgi --ini=/home/tootcloud/TootCloud/uwsgi.ini
    ExecReload = /bin/kill -s HUP ${MAINPID}
    KillSignal = QUIT
    
    [Install]
    WantedBy = multi-user.target
```

`$ sudo systemctl daemon-reload`

`$ sudo su tootcloud`

`$ git clone https://github.com/pyenv/pyenv.git .pyenv`

`$ vi .bashrc`
```
    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="$PYENV_ROOT/bin:$PATH"
    # PATH 環境変数を操作するので、~/.bashrc の最後に記載する。
    eval "$(pyenv init -)"
```

`$ source ~/.bashrc`

`$ pyenv install 3.7.3`

`$ pyenv global 3.7.3`

`$ pip install pipenv`

`$ git clone https://github.com/mayaeh/TootCloud.git`

`$ cd TootCloud`

`$ cp config.py.sample config.py`

`$ python`
```
    >>> import os
    >>> os.urandom(24)
    >>> quit()
```

`$ vi config.py`

`$ pip install uwsgi`

`$ vi uwsgi.ini`
```
    [uwsgi]
    socket = /tmp/uwsgi.sock
    chmod-socket = 666
    chdir = %d
    module = main:app
    master = 1
    processes = 2
```

`$ pip install -r requirements.txt`

`$ pip install matplotlib`

`$ exit`

`$ sudo systemctl start tootcloud`

