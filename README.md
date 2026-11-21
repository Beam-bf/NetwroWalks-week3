# NetwroWalks-week3

# Week 3 – Project Module 1: Password Cracking with John the Ripper

**Internship:** NetworkWalks – Cybersecurity & Ethical Hacking  
**Module:** Week 3 | Project Module 1  
**Lab:** Password Cracking with John the Ripper (JTR)  
**Operating System:** Windows  
**Tools:** John the Ripper 1.9.0 Jumbo, Johnny GUI, PowerShell, PDF Hash Extractor

---

## 1. Introduction

This lab focuses on password recovery using John the Ripper (JTR) and Johnny, its graphical user interface. The purpose of the exercise was to understand how password-cracking tools analyze password hashes and perform dictionary-based attacks.

The assigned task involved recovering the password of a protected PDF file using JTR John and JTR Johnny on a Windows computer.

During this exercise, I installed and configured both tools, extracted password hashes from protected PDF files, performed wordlist-based password recovery attempts, and investigated several configuration issues.

All testing was conducted in an authorized educational environment.

---

## 2. Objectives

The objectives of this lab were to:

- Install and configure John the Ripper on Windows.
- Install Johnny GUI and connect it to John the Ripper.
- Extract password-verification data from a protected PDF document.
- Save the extracted PDF hash in a text file.
- Perform a dictionary attack using John the Ripper and Johnny.
- Analyze password recovery results.
- Troubleshoot incorrect hash formats, file paths, and wordlist configurations.

---

## 3. Tools and Environment

| Tool | Purpose |
|---|---|
| John the Ripper 1.9.0 Jumbo | Password hash auditing and recovery |
| Johnny GUI | Graphical interface for John the Ripper |
| Windows PowerShell | Executing commands and verifying results |
| PDF Hash Extractor | Extracting password-verification data from PDFs |
| Notepad | Saving extracted hashes and password wordlists |
| Protected PDF files | Targets for the authorized lab exercise |

---

## 4. Practical Implementation

### Step 1: Installation of John the Ripper

I downloaded the Windows 64-bit version of John the Ripper Jumbo and extracted the installation files.

The executable was located inside the `run` folder. I navigated to the correct directory using PowerShell and verified that John the Ripper was functioning.

**Command used:**

```powershell
.\john.exe --test=5
```

![John the Ripper Installation](screenshots/01-jtr-installation.png)

*Figure 1: John the Ripper installation files and verification using Windows PowerShell.*

**Observation:** John the Ripper executed successfully after locating the correct executable directory.

---

### Step 2: Installation and Configuration of Johnny GUI

I installed Johnny GUI to perform password-cracking operations through a graphical interface.

After launching Johnny, I opened the Settings section and selected the `john.exe` executable from the extracted John the Ripper folder.

Johnny successfully detected the installed version:

```text
John the Ripper 1.9.0-jumbo-1 OMP
```

![Johnny GUI Configuration](screenshots/02-johnny-configuration.png)

*Figure 2: Johnny GUI configured with the John the Ripper executable.*

**Observation:** Johnny successfully recognized the JTR executable, confirming that both applications were connected.

---

### Step 3: Preparation of the Password-Protected PDF

The password-protected PDF document was prepared for the password recovery exercise.

I also created a separate password-protected PDF with a different password to conduct additional testing.

![Password Protected PDF](screenshots/03-protected-pdf.png)

*Figure 3: Password-protected PDF document used for the recovery exercise.*

**Observation:** The documents required password verification before allowing access.

---

### Step 4: Extraction of the PDF Hash

I used the PDF Hash Extractor to obtain the password-verification data from the protected PDF.

The PDF was uploaded to the extractor, and the generated hash was copied for further analysis.

The extracted data began with the PDF hash identifier:

```text
$pdf$
```

![PDF Hash Extraction](screenshots/04-pdf-hash-extraction.png)

*Figure 4: Extraction of password-verification data from the protected PDF document.*

**Observation:** The extracted data was presented in a PDF-specific hash format suitable for testing with John the Ripper.

---

### Step 5: Saving the Extracted Hash

The extracted PDF hash was copied into Notepad and saved as a text file.

Separate hash files, including `Hash1` and `Hash2`, were prepared for the PDF password recovery exercises.

I ensured that the extracted hash was saved without unrelated text or additional characters.

![Saved PDF Hash](screenshots/05-saved-hash.png)

*Figure 5: Extracted PDF hash saved in a text file for use with John the Ripper.*

**Observation:** Saving the hash in a text file allowed the password-verification data to be loaded into Johnny and John the Ripper.

---

### Step 6: Loading the Hash into Johnny

I opened Johnny and selected **Open password file**.

The saved PDF hash text file was loaded into the application.

Next, I opened **Options** and selected the PDF hash format.

**Configuration:**

- Input file: Extracted PDF hash text file
- Hash format: `PDF`
- Attack mode: `Wordlist`

![PDF Hash Format Configuration](screenshots/06-johnny-pdf-format.png)

*Figure 6: Johnny GUI configured to process the extracted PDF password hash.*

**Observation:** Selecting the appropriate hash format was necessary for John the Ripper to interpret the extracted PDF data correctly.

---

### Step 7: Verifying the PDF Hash Using PowerShell

Before continuing with the graphical interface, I used PowerShell to verify that John the Ripper could recognize the PDF hash.

**Command used:**

```powershell
.\john.exe --format=PDF --wordlist="C:\Users\bamib\Downloads\practice-words.txt" "C:\Users\bamib\Downloads\Hash\Hash2.txt"
```

The console displayed:

```text
Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64])
Cost 1 (revision) is 3 for all loaded hashes
Will run 16 OpenMP threads
```

![PDF Hash Verification](screenshots/07-pdf-hash-verification.png)

*Figure 7: John the Ripper successfully recognizing the extracted PDF hash.*

**Observation:** The hash was successfully loaded, confirming that the extracted PDF hash was recognized by JTR.

---

### Step 8: Performing the Wordlist Attack

I configured a dictionary attack in Johnny by selecting the **Wordlist** attack mode.

A text file containing candidate passwords was provided as the wordlist.

The attack was started to test the candidate passwords against the extracted PDF hash.

The console output included:

```text
Warning: Only 6 candidates left, minimum 16 needed for performance.

0g 0:00:00:00 DONE

Session completed
```

![PDF Wordlist Attack](screenshots/08-pdf-wordlist-attack.png)

*Figure 8: Wordlist attack performed against the extracted PDF hash using Johnny.*

**Observation:** The attack completed after processing the selected password candidates. However, no matching password was recovered during the recorded Hash2 attempt.

---

### Step 9: Additional SHA-256 Password Recovery Test

To verify that John the Ripper and Johnny were functioning correctly, I conducted an additional password recovery test using a self-generated SHA-256 hash.

The practice password was:

```text
hello
```

Its SHA-256 hash was:

```text
2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
```

A wordlist containing the practice password was created and used with John the Ripper.

**Command used:**

```powershell
.\john.exe --format=Raw-SHA256 --wordlist="$env:USERPROFILE\Downloads\practice-words.txt" "$env:USERPROFILE\Downloads\practice-hash.txt"
```

The output showed:

```text
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])

hello            (?)

1g 0:00:00:00 DONE

Session completed
```

![SHA256 Password Recovery](screenshots/09-sha256-cli-success.png)

*Figure 9: Successful recovery of the practice SHA-256 password using John the Ripper.*

**Observation:** The password `hello` was successfully recovered, demonstrating that John the Ripper could perform a dictionary attack when the correct password was present in the wordlist.

---

### Step 10: Successful Password Recovery Using Johnny

The SHA-256 practice hash was also tested through Johnny GUI.

Initially, Johnny used the incorrect GOST hash format. After changing the format to `Raw-SHA256` and starting a new attack, the password was recovered.

Johnny displayed:

```text
100% (1/1: 1 cracked, 0 left) [format=Raw-SHA256]
```

![Johnny Successful Recovery](screenshots/10-johnny-success.png)

*Figure 10: Johnny GUI successfully recovering one SHA-256 practice password hash.*

**Observation:** The successful result verified that the graphical interface was correctly configured and capable of performing password recovery.

---

## 5. Challenges Encountered and Troubleshooting

### 5.1 Executable Not Found

**Problem:** PowerShell initially displayed an error indicating that `john.exe` was not recognized.

**Cause:** The command was executed from an incorrect directory.

**Solution:** I located the `run` folder containing the executable and navigated to that directory before executing JTR commands.

### 5.2 No Password Hashes Loaded

**Problem:** John the Ripper displayed:

```text
No password hashes loaded
```

**Cause:** The initial SHA-256 practice file was incorrectly created due to two PowerShell commands being combined.

**Solution:** I recreated the hash file and saved the correct SHA-256 hash separately.

### 5.3 Incorrect Hash Format

**Problem:** Johnny initially used the `GOST` hash format while the practice hash was SHA-256.

**Solution:** I selected `Raw-SHA256` and started a new attack to apply the corrected configuration.

### 5.4 Incorrect Wordlist Path

**Problem:** Some PDF attack sessions ended immediately without processing password candidates.

The Console log revealed an incorrect wordlist path:

```text
--wordlist=C:\Users\bamib\Downloads\Hash\Hash2.txt
```

**Cause:** The extracted PDF hash file was mistakenly selected as the wordlist.

**Solution:** I replaced it with the correct password wordlist:

```text
C:\Users\bamib\Downloads\practice-words.txt
```

After this correction, the program processed password candidates.

![Johnny Troubleshooting](screenshots/11-johnny-troubleshooting.png)

*Figure 11: Johnny Console log used to identify and troubleshoot incorrect attack configuration.*

### 5.5 PDF Password Not Recovered

**Problem:** The PDF hash was recognized, but the recorded wordlist attack did not recover the password.

**Observation:** The attack completed with zero successful recoveries.

**Possible causes:** The actual password might not have matched the tested candidates, the extracted hash might not have corresponded to the intended PDF, or the PDF password-verification data might have differed from expectations.

The precise cause was not confirmed during the recorded testing.

---

## 6. Results

| Activity | Result |
|---|---|
| JTR installation | Successful |
| Johnny GUI configuration | Successful |
| PDF hash extraction | Completed |
| PDF hash recognition | Successful |
| Wordlist attack against Hash2 | Completed without password recovery |
| SHA-256 dictionary attack | Successful |
| SHA-256 password recovered | `hello` |
| Johnny SHA-256 recovery | 100% — 1/1 hash cracked |
| Troubleshooting | Incorrect format and wordlist selection identified and corrected |

---

## 7. Security Findings

The exercise demonstrated the following concepts:

1. **Hash recognition is essential:** The correct hash format must be selected before password recovery can proceed.

2. **Dictionary attacks depend on password candidates:** A password can be recovered if an appropriate candidate is tested successfully.

3. **Password complexity affects recovery:** Strong, unpredictable passwords reduce the effectiveness of small dictionary attacks.

4. **Incorrect tool configuration affects results:** Selecting the wrong hash format or wordlist can cause attacks to fail even when the tool is properly installed.

5. **Successful hash loading does not guarantee recovery:** John the Ripper may recognize a hash without finding a matching password.

---

## 8. Conclusion

This lab provided practical experience using John the Ripper and Johnny GUI for password auditing and recovery.

I successfully installed and configured both tools, extracted PDF password hashes, verified the PDF hash format, performed dictionary attacks, and resolved several configuration issues.

The SHA-256 practice exercise was completed successfully, with the original password recovered using both PowerShell and Johnny.

During the PDF exercise, the extracted hash was successfully recognized by John the Ripper, and a wordlist attack was performed. However, the recorded Hash2 attempt did not recover the PDF password.

Overall, the lab strengthened my understanding of password hashes, wordlist-based attacks, PDF password protection, and troubleshooting password-auditing tools.

---

**NetworkWalks Cybersecurity Internship – Week 3, Project Module 1**
