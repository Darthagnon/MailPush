# MailPush

This is a **KUAL** plugin for Kindle, which implements a book-by-email push function similar to Amazon's **Send-to-Kindle**, but does not rely on Amazon's `@kindle.com` email service, and can use any email. Before using this plugin, please make sure you have jailbroken your device and installed [**KUAL** and **Python3**](https://www.mobileread.com/forums/showthread.php?t=225030). As the main body of this plugin is based on the `Python3` standard library, the scripts in the `src` folder are actually cross-platform and can run on any operating system and platform where `Python3` is installed.

## See also
- For English users, there is a nice [Reddit post](https://old.reddit.com/r/kindle/comments/uvp41l/howto_email_kfx_books_from_calibre_to_a/)
- для русскоязычных пользователей здесь есть [Russian fork](https://github.com/DarkAssassinUA/MailPushRU) 
- [Python 3.9+ for Win7](https://github.com/adang1345/PythonWin7)
- [KOReader](https://github.com/koreader/koreader) is the best ereader software for Kindle.

## Features
- Supports sending files to Kindle via email attachments.
- Supports sending files to Kindle via a download link in an email. This is sometimes more convenient and allows you to bypass the mailbox file size limit.
- Supports sending files as ZIPs. The plugin will automatically decompress `zip`, `tar`, `gztar`, `bztar` and other compressed package formats.
- Supports specifying the path or file name to save the file in the email/on the device
- Unlike Amazon's official service, MailPush does not have the concept of "approved senders" or other whitelists. You can send files to yourself from any mailbox.
- Unlike Amazon's official service, MailPush supports sending files of any format to any directory and is not limited to books. The plugin is filetype-agnostic and will not perform any format conversions except for decompressing ZIPs.

## Disclaimer
There may be some security risks here (for example, firmware upgrades files can be pushed via MailPush), so you should use a non-public mailbox with a relatively complex name. Alternatively, you can set a stricter path for `root` in the `config.json` file, and files sent by email will not be allowed to be downloaded outside the `root` directory and its subdirectories. `root` defaults to the Kindle USB disk root directory (`/mnt/us/`), so please modify it with caution, e.g. to `/mnt/us/ebooks/`. Do not share your MailPush inbox with anyone; the authors do not encourage piracy by distribution and are not responsible for any issues caused by using this tool. MailPush is provided as-is, without any guarantee of being fit for purpose.

## Installation and configuration
1. Register a new email account. Many email accounts have many restrictions and cumbersome configurations, or strict spam filters. It is recommended to use an Outlook email account, which can simplify subsequent settings. Do not use a password shared with any other account, as MailPush stores it in plaintext!
2. Enable `IMAP` in the email mailbox settings. The exact setting will vary between email providers. For example, IMAP is enabled by default on Outlook email accounts and no further settings are required, whereas with a newly registered QQ email account you must wait 14 days before you can enable IMAP. Some email accounts (such as Yahoo, Google, QQ, Mail.ru) also require you to generate a special "app-specific password" (authorization code) to log in with 3rd-party email clients like MailPush. Using a regular password will fail to log in and the plugin will not work properly. Some email accounts may also have geographic or language barriers due to national firewalls.
3. `git clone` this project or go to the [Releases page](../MailPush/releases) to download the compressed package and unzip it to any directory on your computer.
4. In the unzipped `MailPush/src` folder, find and edit the `config.json` file.
	- Change `user` to the new email account you just registered, e.g. `joe.bloggs.kindle1234@gmail.com`
	- Change `password` to the login password (or "app-specific password" or IMAP authorization code). Again, because it is stored in plaintext, do not use a password common to any other account!
	- Change `host` and `port` to the IMAP host and port of your email service provider. Please refer to the comparison table at the end of the article, or your provider's documentation.
	- Modify other parameters as needed. `downloaddir` is the default download path (recommended `/mnt/us/ebooks/downloads` if using KUAL and KOReader); `maxage` is the age of an email in days, after which it will not be downloaded; `maxnum` is the maximum number of emails to download at a time.
5. Copy the `MailPush` folder to the `/extensions` directory in the root directory of your Kindle device via USB.
6. Safely eject your Kindle. MailPush will now be available in your KUAL menu.

## How to use
1. Use another email address to send an email to the email address you filled in `config.json`
	- You can choose to add any attachment (e.g. ebooks, photos)
	- Both the subject and the body can be empty
	- A line in the subject or body can be a file download link, or multiple: 
	- Multiple links are separated by spaces, line-breaks (1 link per line), or pipes `|`, or enclosed by inequalities `<` and `>` respectively. Commas `,` and semicolons `;` are not supported. 
	- A line in the subject or body can start with the `saveto` keyword to specify the path or file name downloaded to Kindle. Multiple file names are separated by line-breaks (1 file name per line), pipes `|`, or enclosed by inequalities `<` and `>` respectively. Multiple file names cannot be separated by spaces, commas `,` or semicolons `;`. The default path is configured by the parameter `downloaddir` in `config.json`, which defaults to `/mnt/us/documents/downloads`. The format is as follows:
		> - `saveto abc.pdf`              # means the first file is saved to /mnt/us/documents/downloads/abc.pdf
		> - `saveto books/`               # means the first file is saved to /mnt/us/documents/downloads/books/, the file name remains unchanged
		> - `saveto /mnt/us/123.epub`     # means the first file is saved to /mnt/us/123.epub
		> - `saveto abc.pdf | ../def.pdf` # means the first two files are saved to /mnt/us/documents/downloads/abc.pdf and /mnt/us/documents/def.pdf respectively
2. Open KUAL on your Kindle and find `MailPush` in the menu. Make sure you are connected to Wifi. Use an update blocker plugin, e.g. **BlockKindleOTA**, alonside MailPush to avoid accidentally updating and losing your jailbreak while on Wifi. Click the `Fetch unread emails` button to download all unread emails, or click the `Fetch all emails` button to get the files in the latest mail. Other email download modes are also provided.
3. After selecting a download mode, wait a little. The books will download.

## Troubleshooting
1. Click the KUAL >> MailPush menu buttons `View log` and `View results` to view the operation log and results. You can also connect your Kindle to the computer via USB and view `log.txt` and `result.txt` in the `extensions/MailPush/` directory.
2. If MailPush prompts `Operation failed`, please check the content in `log.txt` first. Check the installation status of `Python3` and the configuration in `config.json` (such as `password`)
3. Manually log in to the email address you filled in `config.json`, check whether the email has been received, and add the sender to the whitelist if necessary. Note that logging in to view will turn unread emails into read emails. `Fetch unread mails` will now ignore these emails; you can click the `Fetch all emails` button to attempt redownloading.
4. If the device clock is wrong it may cause the connection to fail. Please set the correct time in the Kindle settings.
5. If the plugin hangs on `Fetching...` at the top of the screen or prompts `Time out`, there may be a network problem. Make sure you have an internet connection, try again later or use another email provider.
6. If it prompts `Operation success` but the file cannot be found, please check the file according to the path in `result.txt` first. If there is no download, you can click the `Fetch junk emails` button and try to find it in spam.
7. If the file has been downloaded but does not appear in your library, please confirm that the file is located in `/mnt/us/documents` and its subdirectories, and confirm that the file type (suffix name) is a format supported by the Kindle. After confirmation, you can try to restart the device. If you're primarily using KUAL and KOReader, you may wish to download books by default to `/mnt/us/ebooks/downloads` or another directory that is not read by the main Kindle software. This will keep KUAL easy to find.

## Appendix: Common mailbox types and host comparison table
MailPush is not limited to just the below email providers; any email provider with IMAP support should work.

|**Email provider**|**Host**|**Port**|**Region**|
|----|----|----|----|
|gmail|imap.gmail.com|993|en-us|
|yahoo|imap.mail.yahoo.com|993|en-us|
|outlook|imap-mail.outlook.com|993|en-us|
|hotmail|outlook.office365.com|993|en-us|
|qq|imap.qq.com|993|cn-zh|
|126|imap.126.com|993|cn-zh|
|163|imap.163.com|993|cn-zh|
|yeah|imap.yeah.net|993|cn-zh|
|sina|imap.sina.com|993|cn-zh|
|mailru|imap.mail.ru|993|ru|
|rambler|imap.rambler.ru|993|ru|
|yandex|imap.yandex.ru|993|ru|

## Credits
- guo-yong-zhi (original author): https://github.com/guo-yong-zhi/MailPush 
- Dark_AssassinUA (4PDA.TO), Russian email support & translation: https://github.com/DarkAssassinUA/MailPushRU, https://bit.ly/dark_kindle
- Darthagnon, English translation improvements: https://github.com/Darthagnon/MailPush

## More of guo-yong-zhi's Kindle plugins
* [**kindle-filebrowser**](https://github.com/guo-yong-zhi/kindle-filebrowser), web file manager
* [**MailPush**](https://github.com/guo-yong-zhi/MailPush), use a third-party email address to push files
* [**BlockKindleOTA**](https://github.com/guo-yong-zhi/BlockKindleOTA), block Kindle from upgrading
* [**KOSSH**](https://github.com/guo-yong-zhi/KOSSH), lightweight SSH server for WiFi connection
* [**ShuffleSS**](https://github.com/guo-yong-zhi/ShuffleSS), shuffle the lock screen pictures
