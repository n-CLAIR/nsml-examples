Quick start guide! Submit을 해보자!


```bash
# examples 저장소를 clone합니다
AL01300657:workspace user$ git clone https://github.com/n-CLAIR/nsml-examples.git
Cloning into 'nsml-examples'...
remote: Counting objects: 30, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 30 (delta 5), reused 30 (delta 5), pack-reused 0
Unpacking objects: 100% (30/30), done.

AL01300657:workspace user$ cd nsml-examples/
AL01300657:nsml-examples user$ ls
examples	kin.md		movie-review.md

# local버전의 nsml을 설치합니다.
AL01300657:nsml-examples user$ pip install git+https://github.com/n-CLAIR/nsml-local.git
Collecting git+https://github.com/n-CLAIR/nsml-local.git
  Cloning https://github.com/n-CLAIR/nsml-local.git to /private/var/folders/zp/6xkph_h94x3fk4dwczy4zg_m0000gn/T/pip-req-build-dierevj_
Requirement already satisfied (use --upgrade to upgrade): nsml==0.0 from git+https://github.com/n-CLAIR/nsml-local.git in /Users/user/anaconda3/lib/python3.6/site-packages
Building wheels for collected packages: nsml
  Running setup.py bdist_wheel for nsml ... done
  Stored in directory: /private/var/folders/zp/6xkph_h94x3fk4dwczy4zg_m0000gn/T/pip-ephem-wheel-cache-vw9ru7j5/wheels/8f/f0/a8/f4d8828b340f609ab58c853b07ad95b3166af75b09d36aaa44
Successfully built nsml


# binary의 nsml을 다운로드합니다.
AL01300657:nsml-examples user$ curl -L -o cli.tar.gz https://github.com/n-CLAIR/File-download/raw/master/nsml/alpha/nsml_client.darwin.amd64.alpha.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   177  100   177    0     0     88      0  0:00:02  0:00:02 --:--:--    80
100 3308k  100 3308k    0     0  1102k      0  0:00:03  0:00:03 --:--:-- 4750k

# extract tar files
AL01300657:nsml-examples user$ tar xvpf cli.tar.gz
x nsml
x README.alpha.txt
x LICENSE-THIRD-PARTY.txt
x examples-alpha/
x examples-alpha/empty


# nsml이 제대로 설치되었는지 확인합니다.
AL01300657:nsml-examples user$ ./nsml --version
Built: 2018-05-11T18:49:36+09:00
Go: go1.10.2
GOARCH: amd64
GOOS: darwin
Protocol: 20180509
Revision: 3f0aa33d78b57da34550ac7194b45ca4894139a2 (not-modified)
Preconfigured connection: alpha.cli.nsml.navercorp.com
AL01300657:nsml-examples user$

# (OPTIONAL)좀더 쉬운 사용을 위해 nsml 을 패스에 등록합니다
AL01300657:nsml-examples user$ export PATH=$PATH:`pwd`

# nsml에 로그인을합니다
AL01300657:nsml-examples user$ ./nsml login
2018/05/16 18:12:54 connecting to alpha.cli.nsml.navercorp.com:18553
2018/05/16 18:12:55 there is no update
GitHub Username: rkdls
GitHub Password: **********
2018/05/16 18:13:05 Welcome to NSML!

# dataset을 확인합니다
AL01300657:nsml-examples user$ ./nsml dataset ls
Name       Size      Last Modified    Author       Description          Tag
---------  --------  ---------------  -----------  -------------------  --------
alpha-kin  11.9 MB   a day ago        rkdls        example of kin data  #example
mnist_new  52.41 MB  a day ago        JinwoongKim
AL01300657:nsml-examples user$

# 등록된 dataset을 이용해서 main.py를 nsml에서 훈련시켜보겠습니다.
AL01300657:nsml-examples user$ ./nsml run -d alpha-kin -e examples/kin/example/main.py
2018/05/16 18:13:59 dataset.py 3.5 KiB
2018/05/16 18:13:59 kor_char_parser.py 3.8 KiB
2018/05/16 18:13:59 main.py 7.6 KiB
2018/05/16 18:13:59 setup.py 1.2 KiB
...........
Session rkdls/alpha-kin/16 is started


# 세션이 잘동작중인지 확인합니다.( -a : 종료된것도 포함합니다)
AL01300657:nsml-examples user$ ./nsml ps
Name                Created      Args     Status    Summary                                    Description
------------------  -----------  -------  --------  -----------------------------------------  -------------
rkdls/alpha-kin/16  seconds ago  main.py  Running   epoch=1, epoch_total=10, train/loss=0.666


# train이 어느정도 진행되었으면 nsml.save(epoch)으로 저장된 checkpoint를 확인해봅니다. (학습 중간중간에 저장된 model)
AL01300657:nsml-examples user$ ./nsml model ls rkdls/alpha-kin/16
Checkpoint    Last Modified    Elapsed    Summary
------------  ---------------  ---------  --------------------------------------------------------------
0             seconds ago      0.293      train/loss=0.6852216747269702, step=0, epoch=0, epoch_total=10
1             seconds ago      4.491      train/loss=0.6656675770211575, step=1, epoch=1, epoch_total=10
2             seconds ago      4.495      train/loss=0.6490269499038582, step=2, epoch=2, epoch_total=10
3             seconds ago      4.519      train/loss=0.6458923265115538, step=3, epoch=3, epoch_total=10
4             seconds ago      4.546      train/loss=0.6402848038210798, step=4, epoch=4, epoch_total=10
5             seconds ago      4.608      train/loss=0.6355513045147284, step=5, epoch=5, epoch_total=10
6             just now         4.608      train/loss=0.6292749407576091, step=6, epoch=6, epoch_total=10
7             just now         4.505      train/loss=0.6230241398313152, step=7, epoch=7, epoch_total=10

# submit 명령어를 이용해서 leaderboard에 나의 점수를 올려봅니다.
AL01300657:nsml-examples user$ ./nsml submit rkdls/alpha-kin/16 7
....................
Score : 0.663592134604
done

# 내가 submit한 모델의 스코어가 어느정도인지 확인해봅니다.
AL01300657:nsml-examples user$ ./nsml dataset board alpha-kin
Owner    Score           Submitted       Session             Model    Args
-------  --------------  --------------  ------------------  -------  -------
rkdls    0.663592134604  a minute ago    rkdls/alpha-kin/16  7        main.py
rkdls    0.661058179607  2 days ago      rkdls/alpha-kin/1   5        main.py
rkdls    0.65822015001   36 minutes ago  rkdls/alpha-kin/5   8        main.py
rkdls    0.65822015001   36 minutes ago  rkdls/alpha-kin/5   8        main.py
rkdls    0.657510642611  36 minutes ago  rkdls/alpha-kin/5   5        main.py
rkdls    0.569227650517  19 hours ago    rkdls/alpha-kin/13  0        main.py


# web에서 public스코어를 확인할수있습니다!
```
