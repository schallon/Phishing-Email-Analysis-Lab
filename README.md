# Phishing Email Analysis Lab
This is a lab report/walkthrough of <a href="https://blueteamlabs.online/home/challenge/the-planets-prestige-e5beb8e545">'The Planet's Prestige'</a> lab from Blue Team Labs. The premise of this CTF lab is to analyze the .eml file from an email exchange to find any suspicious/malicious intentions behind the attachments, message, or means of communication used.
___________________________________________________________________________________________________________________________________________

# Lab walkthrough with screenshots

When opening the .eml provided in Notepad++, the following information is shown as in the screenshot below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/ca413343-0c88-4047-ad6f-f55cd39ac567)

There are a few key things that are underlined to look further into. First, the email was sent to themajoronearth@gmail.com from the billjobs@microapple.com email address. However, the reply to email address is negeja3921@pashter.com which does not line up with the email address that the email says it is sent from. Finally, spf is set to fail coming from the emkei.cz email service given along with the 93.99.104.210 ip address that Google.com does not designate as a permitted sender. These are a few indicators that something is up with this email and it needs to be investigated further.

Below is a screenshot of the first BOUND item in .eml file. From the Content tags above the base64 encoded message, it is plain text.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/a2c5bb6a-7fed-4ee8-8e2f-2c1768f94a17)

When decoded, the plaintext comes out to the following message as shown in the screenshot below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/76c3f417-981f-40b5-8d6b-02fe7ed03caa)

The second BOUND item in .eml file appears to be a PDF/Application file named PuzzleToCoCanDa.pdf as shown in the screenshot below followed by the decoded information.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/b033b0d3-8269-48e0-9026-677c692caf72)

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/8806391f-77a0-4535-be02-257186e185c8)

Looks like a bunch of jibberish, but when converted to Hexidecimal as shown below, the file signature did not line up with 25 50 44 46 which is the file signature of PDFs. Instead it has the file signature of 50 4B 03 04, which is the file signature for ZIP files.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/930fbff6-1bd7-4b8f-81e7-7381005faf0d)

After saving the decoded information of the PuzzleToCoCanDa.pdf, which is actually a ZIP file, to a zip file, it was transferred it to a Windows 10 virtual machine in VMware Player. Below are the files that were contained in that zipped folder: DaughtersCrown, GoodJobMajor, and Money.xlsx. The Money.xlsx was a hidden file that became visible after setting hidden files to visible in File Explorer.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/9983efd1-ce91-44bb-80e3-4eae4013baaf)

Next is to use a program called HxD to view the Hexidecimal values of all of the files to figure out what kind of files they are. But before doing so, as a generic safety precaution, the virtual machine had shared folders and the network adapter disabled to prevent any possible interaction with the host machine or the internet. Once checked to make sure that the virtual machine was unable to reach Google.com, the DaughtersCrown file was opened in HxD as shown below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/1a05bfd9-52a3-4c21-8a7b-3776da3a2040)

The highlighted portion of the hexidecimal for the file is the signature, FF D8 FF E0. This signature indicates that the file is a JPEG. So after adding .jpeg to the end of the DaughtersCrown file making it DaughtersCrown.jpeg, when opened it shows the image below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/d5551afa-607b-4f51-a8eb-80bd7473c29b)

When the GoodJobMajor file was opened in HxD it porduced the hexidecimal values in the image below. The file signature for this file was 25 50 44 46, which as previously mentioned, indicates that it is a PDF file.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/3330750d-5b8c-469f-bbe6-133273d03327)

After adding .pdf to the end of the file name and opening it, it showed the content in the screenshot below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/4ff13bb3-86b5-40f8-8bc9-c6feca2a588b)

To make sure the Money.xlsx file is actually an xlsx file, it was put into HxD aswell which produce a file signature of 50 4B 03 04, which is the signature that is associated with xlsx files. When opended in OpenOffice, the following content was shown.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/31fbb3f0-2b35-4085-af80-98d700298e6e)

As mentioned in the B3 cell, "Whatever you have seen or read till now is fake", which is in line with what has been discovered thus far. However, there is also a Sheet3 that may have hidden information. When opened, it appears as a blank sheet with no information. After removing the formatting in place, C4 has a Base64 encoded message as shown in the screenshot below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/17b9dc90-448d-4af4-82b0-155d4e934bc3)

When decoded, it gives the location of the bad actor as shown in the screenshot below.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/068b2d9c-979d-4ded-a675-73c8fb2a7f07)

Now looking at the challenge questions given by the lab, all of the information to complete it has been found except a name. When putting in Bill Jobs from the sending email address, it shows as incorrect.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/5b49782b-057e-4501-859c-dd89a34343f8)

To find the actual name of the bad actor, an exif tool may be able to find the author of files from the zipped folder. After using said exif tool, the GoodJobMajor.pdf file has Pestero Negeja in the Author field. When input into the challenge question, it was accepted.

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/57151764-1e77-434e-a57a-81b3a123d6c5)

![image](https://github.com/schallon/Phishing-Email-Analysis-Lab/assets/55300128/cae199e3-2748-434f-8f74-ad0d2b37e919)

Most of the remaining questions are able to be answered directly from the .eml file given at the beginning of the lab. For question 1, the email service can be found in the same area with all of the spf failures. More specifically in this situation, it is found in the Received: from localhost area where emkei.cz is given along with the ip address associated with it. For question 2, the Reply-To email address that was highlighted earlier, negeja3921@pashter.com, is the correct answer. For question 3, the PuzzleToCoCanDa.pdf file attachment provides the answer as it was actually a .zip file. For question 5, the location that was found from the Money.xlsx file on Sheet3 showed that the location was The Martian Colony, Beside Interplanetary Spaceport. Finally, the probable C&C domain to control the attacker's autonomous bots was assumed to be the same domain given in the Reply-To email address, that being pashter.com.
_________________________________________________________________________________________________________________

# Review/Learnings

This lab gave a lot of insight into many different tools, both online and offline executables, that can be used to decipher emails to find potentially suspicious activity. For example, one of the first things that stood out was the SPF failure. There are many other email security measures such as DKIM and DMARC that would serve as a similar key indicator that something could be malicious with given emails. There are also many free and easliy accessible tools that can be used to break down pieces of information into smaller and more digestible parts to find more clues into if they are malicious or not.

What I learned:
- General formatting of .eml files and key areas/items to look at for signs of malicious actors
- All files have signatures, that can be seen in hexidecimal, that show what type of file that they are
- Attached files may appear as different types of files as than they actually are
- There are many ways to hide information in plain sight(formatting, hidden files, etc.)
