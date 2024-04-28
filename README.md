# Phishing Email Analysis Lab
This is a lab report/walkthrough of <a href="https://blueteamlabs.online/home/challenge/the-planets-prestige-e5beb8e545">'The Planet's Prestige'</a> lab from Blue Team Labs. The premise of this lab is to analyze the .eml file from an email exchange to find any suspicious/malicious intentions behind the attachments, message, or means of communication used.

What I learned:
- General formatting of .eml files and key areas/items to look at for signs of malicious actors
- Attached files may appear as different types of files as they actually are
- 
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

