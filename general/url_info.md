###### tags: `通用技術`

# 網址介紹

### 網址的組成


一般的網址長這樣：`http://www.youtube.com:80/path/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument`

1. **通訊協定** \(protocol\) 又稱為 **scheme**，常見的是 `http` 或 `https`
2. **第三層網域名稱**，上述 `www` 的部分，又稱為**子網域** \(subdomain\) 或**主機名** \(host\)，常見的是 www \(全球資訊網\)、FTP \(檔案傳輸協定\)、mailto \(電子郵件傳輸\)
3. **第二層網域名稱**，上述 `youtube` 的部分，又稱為**網域名稱** \(domain name\)，此部份是我們自己決定的，會影響 SEO、網站品牌、使用者識別等因素
4. **頂級網域名稱**，上述 com 的部分，常見的是 com \(公司行號或營利單位\)、gov \(政府機關\)、edu \(學術機構\)、net \(網路組織\)
5. **國別頂級網域**，代表國家，台灣以 tw 結尾
6. **連接埠** \(port\) 預設是 80 不用填
7. **網頁路徑** \(path\) 上述 `/path/myfile.html`的部分
8. **參數** \(parameters\) 上述`?key1=value1&key2=value2`的部分
9. **錨點** \(anchor\) 上述`#SomewhereInTheDocument`的部分，可以對應到網頁 HTML 中的 id 或 name

### IP 位址 \(Internet Protocol Address\)

> 網際網路協定位址，由四組小於或等於 255 的十進位數所組成，具有唯一性

### 網域名稱 \(Domain Name\)

> 對於人類來說，較難記住一長串的 IP 位置，因此創造了 Domain Name 作為電腦在網路上的名子，這個名子是獨一無二的

### DNS \(Domain Name Service\)

> DNS 網域名稱系統，可以做名稱解析的服務，也就是可以透過 IP 位址查詢網域名稱，或透過網域名稱查詢 IP 位置

### SSL 憑證 \(安全通訊協定\)

> 瀏覽器和網路通訊時加密，保護信用卡等機密資訊不被攔截，且是 SEO 重要資訊之一

### 連接埠 \(port\)

> 扮演通訊的端點，每個 port 都會與 IP 與通訊協定關聯，用途為標示伺服器上提供特定網路服務的行程，如 http 預設為 80 port，https 預設為 443 port

