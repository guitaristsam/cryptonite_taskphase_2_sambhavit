# tunn3l v1s10n
**Flag:picoCTF{qu1t3_a_v13w_2020}**
## My thought process and approach to the challenge:
Tried opening the file and nothing made sense so I tried to open it in HexEdit.
![image](https://github.com/user-attachments/assets/fbfcc32a-17b1-43db-94f3-a8c4071e9bbb)
Saw nothing wrong at first, but then I saw that I'd seen `BA` and `BA` again in the top row, so I tried researching it.        
After a lot of research, I learned that the file starts with 42 4D, which indicates a BMP file (Bitmap file format).                              
So, I tried saving it as a BMP, but that also didn't work.

So, I edited the start by comparing it with another BMP file.
![image](https://github.com/user-attachments/assets/f9daae8c-ed19-4622-bf1b-bae300268c46)
Then I saved it and opened it but got
![image](https://github.com/user-attachments/assets/d18f3276-8f99-4658-88fa-207386fd5f6c)
So, I knew I was on the right path and just had to think a bit more.
Then I tried the Exif tool to try and see if I missed anything.
![image](https://github.com/user-attachments/assets/15eeb154-1aa8-45ae-8009-566792432f38)
Using this I got the real height and width of the image.    
After a few trial and error by changing the image height, I figured it out.   
![image](https://github.com/user-attachments/assets/7f1e1f77-d54c-41a6-98c5-17b836355a26)
Using this I got:
![image](https://github.com/user-attachments/assets/00b0fb27-ac90-4dfa-afca-b7423e34a13d)
This gave me the flag: **picoCTF{qu1t3_a_v13w_2020}**

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
### 1. Learned More About File Formats and Headers
- **Files have unique headers identifying their formats.**  
- Recognized `42 4D` as a BMP file identifier (too much researching just to figure this out).

### 2. Learned How to More Efficiently Use HexEdit
- **Gained experience using HexEdit to modify file headers.**  
- Practiced identifying anomalies by comparing with valid BMP files.

### 3. Learned How to Efficiently Use Exif Tools to See Metadata
- **Discovered the Exif tool's utility in extracting metadata from images.**  
- Learned to use image height and width to correct file formatting and uncorrupt files.

### 4. Debugging Techniques
- **Learned the importance of systematic trial and error when resolving file format issues.**      
 
 ---
 
##  Incorrect tangents I went on while solving :
### 1. Attempting to save the file directly as a BMP
- Initially assumed that saving with a `.bmp` extension would solve the problem, which led nowhere.

### 2. Ignoring metadata
- Spent time trying random fixes instead of using tools like `Exif` to get precise information.

### 3. Downloading GIMP to "fix the image"
- Tried opening and "repairing" the image using GIMP, but the software couldn't process it due to the corrupted header.

### 4. Editing unrelated parts of the file
- Changed parts of the file other than the header, which only created more errors when trying to open it.

### 6. Assuming the issue was beyond repair
- Spent time questioning if the file was corrupted beyond recovery and not part of the challenge, which delayed progress.

### 7. Searching for hidden data in the file
- Spent unnecessary time looking for embedded steganographic data in the first picture or encoded strings instead of focusing on fixing the image structure.

