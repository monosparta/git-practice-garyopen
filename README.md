# Git-基本介紹
Linux kernel在2002年前是使用補丁(patch)來進行維護，後來Linux決定使用BitKeeper來做版本控制(有引發部分自由軟體社群的不滿)，但在2005年時一個逆向工程的紛爭，使Bitkeeper的公司收回了給Linux的免費授權，所以Linux的創始者-Linus Benedict決定自己打造一個版本控制系統-Git。
# 為什麼要做版本控制、版本控制有哪些？
一個大型專案如果不做好版本控制，目前上線版本跟開發版本的差別、程式越改越多BUG我想重新再來、這功能是甚麼時候加的啊，就像圖書館的書沒有做好分類一樣，下場會發常可怕，所以為了方便工程師管理專案，版本控制是一定要做好的。

版本控制主要分為集中式(centralized)與分散式(distributed):
||集中式|分散式|
|:-:|:-:|:-:|
|儲存庫|由一個伺服器管理儲存庫|每個工程師都有自己的儲存庫|
|作業效率|一次只能一位工程師進行作業(上鎖)|工程師們都能個人作業|
|檔案統一性|百分之百同步| 各個儲存庫的進度不同|
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
# 本機上的Git(從add到commit)
<img width="427" alt="未命名" src="https://user-images.githubusercontent.com/32414355/154188453-3f2c743b-bb9c-4c44-8aa2-6dbef4dde56c.png">

※add檔案的幾種語法
# 提交(commit)
每當工程師完成一項「工作」，都會做一次commit為其做註解(message)並傳入儲存庫，至於「工作」的定義還是看團隊或個人習慣。

	$ git commit -m "commit訊息"
※ 萬一太早commit有些檔案忘記add的話，add完它們後 --amend，就會取代上一次的commit! 

	$ git commit --amend # 要更改message後面加 -m "新message"
   
※查查branch上的commit資訊(越上面越新)

	$ git log # 退出log按Q
 <img width="427" alt="log" src="https://user-images.githubusercontent.com/32414355/154188248-365faa66-49c1-4292-90a2-e9f446faf55c.png">

※查的更仔細(有哪些commit是我做的、這個功能有哪些commit)  

	$ git log --oneline [條件] # 條件可以是--grep(找關鍵字)、--author(找作者)、--since(何時開始)
 <img width="355" alt="commit" src="https://user-images.githubusercontent.com/32414355/154188022-04ac6fd4-dd43-441e-8d12-d14874dc1207.png">

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

<img width="586" alt="分支" src="https://user-images.githubusercontent.com/32414355/154198587-4937537c-aa72-4bcb-ad11-8a1967f262b8.png">

	
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

2.強制推送，覆蓋途中的commit(別亂用，會被找去喝茶)

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
	$ git remote # 檢視遠端設定   
	$ git status # 檢視目前狀態
  
### 控管  
  	$ git add * # 控管檔案
	$ git rm --cached [不要控管的檔案] # 解除控管   
### 推送(push)    
### 拉(pull)  

 	$ git pull # git fetch + git merge  
	$ git fetch # 上線並把資料抓下來  
	$ git merge # 將資料合併   
### 分支 (branch)  

	$ git branch # 檢查目前有哪些分支(*為當前所在的分支)  
	$ git branch [分支名稱] # 新增一個該名稱分支  
	$ git checkout [分支名稱] # 切換分支  
	$ git branch -m [原分支名稱] [更改後分支名稱] # 幫分支換名稱  
	$ git branch -d [分支名稱] # 刪除某個分支
# GitFlow 介紹
#  我的小小狀況
### 1. Please tell me who you are?  
<img width="432" alt="who_you_are" src="https://user-images.githubusercontent.com/32414355/154187923-6f2a43e2-8a6c-4f8f-bf4c-f70cb1b06c5f.png">

A:第一次使用git時，需要設定使用者  

	git config --global user.name "你的名字"  
	git config --global user.email "你的電子郵件"
※如果突然有個Repository要不同的使用者?

	git config --local user.name "你的名字"
	git config --local user.email "你的電子郵件"

### 2. push時github登入一直失敗"Logon failed, use ctrl+c to cancel basic credential prompt."
A:更新git至最新版(或帳密真的輸錯= =)


