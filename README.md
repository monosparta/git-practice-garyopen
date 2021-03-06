# Git-基本介紹
Linux kernel在2002年前是使用補丁(patch)來進行維護更新，後來Linux決定使用BitKeeper來做版本控制(有引發部分自由軟體社群的不滿)，但在2005年時一個逆向工程的紛爭，讓BitMover收回了Linux使用BitKeeper的免費授權，所以Linux的創始者-Linus Benedict決定自己打造一個版本控制系統-Git。
# 為什麼要做版本控制、版本控制有哪些？
一個大型專案如果不做好版本控制可能會發生哪個是上線版本哪個是開發版本、程式越改越多BUG我想重來的版本是哪個、這功能是甚麼時候加的啊，就像圖書館的書沒有做好分類一樣，下場會非常可怕，所以為了方便工程師管理專案，版本控制是一定要做好的。

版本控制主要分為集中式(centralized)與分散式(distributed):
||集中式|分散式|
|:-:|:-:|:-:|
|儲存庫|由一個伺服器管理儲存庫|每個參與者都有自己的儲存庫|
|作業效率|一次只能一人進行作業(上鎖)|參與者們都能個人作業|
|檔案統一性|高同步率| 各個儲存庫的進度不同|
|代表|Git、Mercurial| CVS、SVN|

# 我的第一次Git
### 1.建立儲存庫(Repository)
在專案資料夾下輸入
		
	$ git init  # 專案裡就會出現Repository(.git檔案)
<img width="382" alt="1" src="https://user-images.githubusercontent.com/32414355/154189087-46cbfc97-afb2-46d3-aa46-f210601d0e42.png">

### 2.將儲存庫裡的檔案進行版本控制 

	$ git add * # 將檔案給git進行控管
	$ git commit -m "my first commit" # 完成第一次commit
<img width="381" alt="1" src="https://user-images.githubusercontent.com/32414355/154189441-fe26e2c7-a04c-4013-b0b1-a4ae717b2e0a.png">

※專案裡如果已經有檔案了，先做一次add和commit最好
### 3.第一次push上github

	$ git remote add origin [github專案的https] # 設定遠端數據庫
	$ git push origin master # 進行推送
<img width="382" alt="1" src="https://user-images.githubusercontent.com/32414355/154189687-8bbbc3f9-10f9-42e8-967e-aad173837c60.png">

※origin是遠端儲存庫(Server)的預設名稱，master是git的初始預設分支(branch)
# 本機上的Git
下圖為新增或更動的檔案從控管(add)到提交(commit)的移動方向

<img width="397" alt="未命名" src="https://user-images.githubusercontent.com/32414355/154200365-918f8985-32ca-4c0f-ae37-d1e84bf9af0d.png">

### 檢視Git目前狀態
檢查目前在哪個分支，此分支上的新增或更動檔案有沒有被控管

	$ git status
	
![status](https://user-images.githubusercontent.com/32414355/154283320-36c165d6-f540-4b39-afbd-3f9e39628d80.png)  

※add檔案的幾種方式

	$ git add * # 所有更新的檔案都控管
	$ git add *.pdf  # 所有更新的某檔案都控管
	$ git add main.html  # 指定控管
	
※解除add控管

	$ git rm --cached [不要控管的檔案] # 解除控管 
	
※改檔案名稱(或暴力法直接改，然後解控舊的，控管新的= =)

	$ git mv 舊檔名.pdf 新檔名.pdf

# 提交(commit)
每當工程師完成一項「工作」，都會做一次commit為其做註解(message)並傳入儲存庫，至於一次「工作」的定義還是看團隊與個人習慣。

	$ git commit -m "commit訊息"
※ 萬一太早commit有些檔案忘記add的話，add完它們後 --amend，就會取代上一次的commit!(也可以直接使用來更改上一次的commend訊息)

	$ git commit --amend # 要更改message後面加 -m "新message"
   
※查查branch上的commit資訊(越上面越新)

	$ git log # 退出log按Q
 <img width="427" alt="log" src="https://user-images.githubusercontent.com/32414355/154188248-365faa66-49c1-4292-90a2-e9f446faf55c.png">

※查的更仔細(有哪些commit是我做的、這個功能有哪些commit)  

	$ git log --oneline [條件] # 條件可以是--grep(找關鍵字)、--author(找作者)、--since(何時開始)
 <img width="355" alt="commit" src="https://user-images.githubusercontent.com/32414355/154188022-04ac6fd4-dd43-441e-8d12-d14874dc1207.png">
 
 ### 標籤(tag)
 為commit貼上標籤並作註解(通常用來標示版本)
 
 	$ git tag # 查詢目前的所有標籤

設定標籤

	$ git tag -a v1.0 -m "version 1.0" # 設定當前commit的標籤(-a)和詳細註解(-m)
	$ git tag -a v1.0 [commit碼] # 指定幫當某個commit做標籤
顯示標籤資訊

	$ git show [標籤] # 秀出指定標籤的資訊
![show](https://user-images.githubusercontent.com/32414355/154285846-e406c798-2585-48dd-a58a-a357d9a58712.png)

設定小標籤

	$ git tag v1.0 # 小標籤(沒有-a)只有它的commit沒有其他額外資料

# 復原的方法
### 重製(reset)
不管是add錯、commit錯、想要把檔案退回到某個commit，總之想要把做錯的東西清除退回，都可以使用reset重新來過，不過還是有些區別。
#### add退回不可控(把檔案給reset)

	$ git reset [要退的檔案] 
#### commit退回(把Commit給reset，主要是調整head的動向)

	$ git reset a4550a7 # 把commit退回指定處(指定commit碼)
	$ git reset a4550a7^ # 把commit退回指定處的上一個commit
	$ git reset HEAD~1 # 數字代表，把commit退回了幾個(1表示退回上一次)

※reset的三兄弟mixed、hard、soft
|--mixed|--soft|--hard|
|:-:|:-:|:-:|
|把head退回至指定處的add前狀態(所以也可以用來取消控制，是reset的預設指令)|把head退回至指定處，並把到後的其他commit先丟回暫存區|把head退回至指定處，並把到後的其他commit清除|
### checkout
並不會做任何的刪改，單純就是移動head的位置，回到指定處

	$ git checkout a4550a7 # 把commit退回指定處(指定commit碼)
	$ git checkout a4550a7^ # 把commit退回指定處的上一個commit
	$ git checkout HEAD~1 # 數字代表，把commit退回了幾個(1表示退回上一次) 
※想要回到分支的最新進度，輸入分支名稱就可以了

	$ git checkout [當前分支]

# 分支 (branch)
有時候一個專案，有不同的功能需要同時進行開發，又不想干擾到彼此，就可以使用分支，最後將其合併即可。
### 檢查分支

	$ git branch # 檢查目前有哪些分支(*為當前所在的分支)  
<img width="346" alt="branch2" src="https://user-images.githubusercontent.com/32414355/154191990-58419107-e901-42b9-9fc2-463d11cc5636.png">

### 新增分支

	$ git branch [分支名稱] # 新增一個該名稱分支 
<img width="347" alt="branch" src="https://user-images.githubusercontent.com/32414355/154192173-352d0cd0-3576-41aa-8e7a-58b3d47e8aa3.png">

※刪除分支

	$ git branch -d [分支名稱] 
### 分支合併
先切換到要併的分支上

	$ git checkout [要併分支]
之後merge要被併的分支

	$ git merge [被併分支]
<img width="367" alt="合併分支" src="https://user-images.githubusercontent.com/32414355/154193558-9c5342cb-665c-429a-a0d6-85803f66f815.png">


※小成果圖

<img width="564" alt="分支" src="https://user-images.githubusercontent.com/32414355/154389082-edb812b3-fd2a-4f2e-a5cb-369dc3e75f29.png">

### Rebase合併
把分支上的commit都複製起來，帶到要併的分支(被帶過來commit的commit碼會變)把它接上去

	$ git rebase [要併分支] # 在被併分支上輸入

![Untitled Diagram drawio (1)](https://user-images.githubusercontent.com/32414355/154393786-6d7b3ff8-2c9f-4310-b6da-103b8372c6bd.png)


# 遠端的推送(push)與拉(pull)
### 推送(push)
將自己本機做完的commit傳送至遠端的儲存庫(EX:Github)。

	$ git push origin master # 將commit推到origin的master分支
	
<img width="342" alt="push" src="https://user-images.githubusercontent.com/32414355/154191469-2d9440e3-36c6-4d30-9652-181a0467dac6.png">
※在第一次的git已經有設定過remote了，如果要再檢查  
	
	$ git remote -v

<img width="305" alt="fetch" src="https://user-images.githubusercontent.com/32414355/154199185-fb20d682-512c-4587-9301-8b9b8c1b8fd5.png">

※好懶我不要一直輸入那麼長push指令

	$ git push -u origin master #　設定完 upstream 後，下次就只需要git push就好

※團隊有人比我早push，造成push不上去的兩種解決方法  
1.先拉再推  

	$ git pull --rebase # 拉下來用rebase合併，如果怕衝突直接pull也可以!
	$ git push # 再推!

2.強制推送，覆蓋途中的commit(別亂用，會被找去泡茶)


	$ git push --force
	

### 拉(pull)
將遠端儲存庫的commit抓來與本機數據庫合併

	$ git pull
![pull](https://user-images.githubusercontent.com/32414355/154189873-c3d6cadb-080b-49cf-a08c-efd130ec6d6f.png)

也可以 pull 拆解成 fetch 再 merge  

	$ git fetch # 從遠端把領先本機當前版本的commit與檔案抓下來(如果本機的commit領先遠端，輸入不會有任何反應)
	$ git merge # 將遠端抓下來的commit與檔案與本機進行合併



※pull跟clone的差別  
clone是將遠端儲存庫建立在本機，pull是更新本機的儲存庫。
# .gitignore 
有些檔案基於安全性，像測試的文件檔、API的KEY和其他不想讓其他人知道的code都會使用.gitignore來防止push。

	直接指定某個檔案->所在資料夾/shy.html

	所有的某類型檔都不能上去->*.pdf
哀我明明有設定，為啥還給我push哭哭(真人真事):  

	$　git rm --cached [不要push的檔案]
※因為原本就被控管的檔案，.gitignore不會生效，要解得解除控管!

# Git 常用指令集統整
### 檢視 

 	$ git log # 檢視commit紀錄  
	$ git remote -v # 檢視遠端設定   
	$ git status # 檢視目前狀態
### checkout

 	$ git checkout [指定commit] # 將專案(head)移至任一指定地點(可以是切換分支，檢視舊commit...等)	
### 控管  

  	$ git add * # 控管檔案
	$ git rm --cached [不要控管的檔案] # 解除控管   
### 提交(commit)
	
	$ git commit -m "commit訊息"
	$ git commit --amend # 取代上一次commit
### 標籤(tag)

	$ git tag # 查詢目前的所有標籤
	$ git tag -a v1.0 -m "version 1.0" # 設定當前commit的標籤(-a)和詳細註解(-m)
	$ git tag v1.0 # 小標籤(沒有-a)只有它的commit沒有其他額外資料
### 重製(reset)

	$ git reset (--mixed) [指定commit或檔案]# 將專案(head)回到指定commit或檔案的未控管(add前)狀態
### 推送(push)    
	
	$ git push origin master # 將commit推到origin的master分支
	$ git push -u origin master #　設定完 upstream 後，下次就只需要git push就好
### 拉(pull)  

 	$ git pull # git fetch + git merge  
	$ git fetch # 上線並把資料抓下來  
	$ git merge # 將資料合併   
### 分支 (branch)  

	$ git branch # 檢查目前有哪些分支(*為當前所在的分支)  
	$ git branch [分支名稱] # 新增一個該名稱分支   
	$ git branch -m [原分支名稱] [更改後分支名稱] # 幫分支換名稱  
	$ git branch -d [分支名稱] # 刪除某個分支
# Git Flow
Git Flow規範主要是透過5大分支來分類commit，降低專案發生commit混亂的問題。
### 長期分支
會一直存在於專案分支上
* master
	- Git預設的初始分支，在Git Flow是用來放可上線版本的commit
	- 不會在這個分支上直接commit，都是透過其他分支合併而來
	- 通常都會有版本標籤
* develop
	- feature們合併的主要分支，不過所有短期分支都有跟它進行合併的機會
	- 有著最新版本的完整feature
### 短期分支
當「工作」完成就會被合併並刪除
* hotfix
	- 從master延伸出來的分支，上線版本突然出現問題時，會開此分支進行熱修
	- 熱修完畢後，會同時跟master和develop合併後刪除
* release
	- 從develop延伸出來準備上線前的測試分支，做上線前的最後修正與準備
	- 測試完畢後，會同時跟master和develop合併後刪除(測試時可能會修正develop時沒注意到問題)
* feature
	- 從develop延伸出來的分支，可能同時存在好多個不同feature的分支
	- 該feature開發完畢就會和develop合併後刪除
#  我的小小狀況
### 1. Please tell me who you are?  
<img width="432" alt="who_you_are" src="https://user-images.githubusercontent.com/32414355/154187923-6f2a43e2-8a6c-4f8f-bf4c-f70cb1b06c5f.png">

A:第一次使用git時，需要設定使用者  

	git config --global user.name "名字"  
	git config --global user.email "電子郵件"
※如果突然有個Repository要不同的使用者?

	git config --local user.name "名字"
	git config --local user.email "電子郵件"

### 2. push時github登入一直失敗"Logon failed, use ctrl+c to cancel basic credential prompt."
A:更新git至最新版(或帳密真的輸錯= =)
