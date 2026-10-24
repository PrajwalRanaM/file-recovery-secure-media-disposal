 File Recovery and Secure Media Disposal

Project Overview
This project documents a practical digital forensics lab investigating deleted-file recovery, data remanence, hash verification, and secure media disposal.

The lab demonstrates that deleting files and emptying the Recycle Bin does not necessarily remove their underlying data from a storage device. It also explores how overwriting free space affects the ability of recovery software to recover deleted files.

Objectives
- Understand the relationship between NTFS metadata, the Master File Table (MFT), and file data clusters.
- Recover permanently deleted test files using Recuva.
- Verify recovered files using SHA-256 hash comparisons.
- Use the Windows built-in 'cipher' command to overwrite free space on a test drive.
- Test file recoverability after overwriting.
- Understand the limitations of software-based sanitisation.

Lab Environment
Environment --> Windows virtual machine 
File recovery tool --> Recuva 
Hash verification --> Windows PowerShell 'Get-FileHash' with SHA-256 
Free-space overwriting --> Windows built-in 'cipher' command 
Target --> Dedicated test drive in the virtual machine 

Methodology
1. File Deletion
Test files were created on the test drive, deleted, and removed from the Recycle Bin. This established the scenario for investigating whether permanently deleted files could still be recovered.

2. Deleted-File Recovery
Recuva was used to scan the test drive and recover the deleted files. All the test files were successfully recovered.
Recovered files were saved separately from the drive being scanned to reduce the risk of overwriting remaining deleted data.

3. SHA-256 Hash Verification
SHA-256 hashes were calculated for the original test files and compared with the hashes of the recovered files.
The hashes matched, providing strong evidence that the recovered files contained the same data as the reference files.

4. Free-Space Overwriting
The Windows built-in 'cipher' command was used to overwrite free space on the test drive. This step investigated whether deleted data that remained recoverable before overwriting could still be recovered afterward.

5. Post-Overwrite Verification
Recuva was used to test recovery again after the overwriting operation. The deleted files could no longer be recovered through the attempted recovery process.
This result demonstrated a change in recoverability under the conditions of the lab. It does not establish that every physical storage area was sanitised or that recovery by every possible method is impossible.

Results
Recovery before overwriting --> All test files recovered 
SHA-256 comparison --> Hashes matched 
Free-space overwriting --> Completed using Windows 'cipher'
Recovery after overwriting --> Recuva could no longer recover the deleted files 

Key Learning Outcomes
- Deleted filesystem metadata and the underlying file contents are separate. Deleting a file does not necessarily erase its data immediately.
- Recovery software can retrieve deleted data when the relevant contents remain available.
- Matching SHA-256 hashes support verification that recovered file contents match a trusted reference.
- Saving recovered files to a separate destination helps preserve the source evidence.
- Free-space overwriting can reduce the recoverability of deleted files on suitable storage media.
- Sanitisation methods must be selected according to the storage technology, sensitivity of the information, and required level of assurance.

Technical Understanding
File Recovery
Deleting a file does not necessarily erase its underlying contents immediately. Depending on the filesystem and storage conditions, recovery software may retrieve data that remains available after deletion.

File Integrity Verification
SHA-256 hashing was used to compare original test files with their recovered copies. Matching hashes provide strong evidence that the compared files have identical contents, but do not independently prove the history, authenticity, or chain of custody of those files.

Media Sanitisation
The Windows cipher /w:E:\ command was used to overwrite free space on the test drive. The subsequent Recuva test showed that the deleted files were no longer recoverable through that particular recovery attempt.

This result is not proof that every physical storage area was sanitised. Media sanitisation requires consideration of the storage technology, inaccessible or remapped sectors, and the level of assurance required.

Key Distinction
- Recovery: investigates whether deleted data remains accessible.
- Hash verification: compares file contents for integrity.
- Sanitisation aims to make data inaccessible according to a defined level of assurance.

These are related but distinct processes, and success in one does not automatically prove success in another.

Limitations
- This was a controlled lab using a Windows virtual machine and test data.
- The recovery result reflects Recuva's behaviour in this experiment. It does not prove that recovery is impossible using every tool or technique.
- The 'cipher' free-space overwrite operation is not equivalent to verified sanitisation of every physical area of an entire drive.
- SSDs, flash storage, inaccessible sectors, and failed media may require different sanitisation methods.
- The results should not be interpreted as a certification of secure media disposal.

Evidence
Screenshots documenting the lab stages will be provided in the screenshots directory.

Commands Used
1. Calculate SHA-256 File Hashes
PowerShell was used to calculate SHA-256 hashes for the original test files and their recovered copies.

Get-FileHash "C:\path\to\file" -Algorithm SHA256

Purpose: Compare the original file's hash with the recovered file's hash. Matching SHA-256 values provide strong evidence that the file contents are identical.

Result:The hashes matched for the files tested in this lab.

2. Overwrite Free Space
The Windows cipher command was used to overwrite free space on the test drive, E:.

cipher /w:E:\

Purpose: Overwrite free space where remnants of deleted files may remain.

Result: After the operation, Recuva could no longer recover the deleted test files in the recovery attempt performed for this lab.

Important limitation: This command targets free space, not every area of the physical drive. The observed result does not prove that all data was irrecoverable using every possible method or that the entire device was securely sanitised.

References
- Lab guide: *Lab: File Recovery and Secure Media Disposal*.
- Microsoft Learn: [cipher command reference](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cipher).
- NIST: [Guidelines for Media Sanitization, SP 800-88](https://csrc.nist.gov/pubs/sp/800/88/r1/final).

