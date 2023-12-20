# Overview 
This repository contains the work and findings from my job simulation experience with ANZ Australia. It was conducted through the Forage platform. The simulation focused on investigating potential cybersecurity threats with ANZ. This is a crucial skill in today's digital landscape where cyber attacks are prevalent.

# Network Traffic Analysis Report

This repository hosts the findings and supporting documents from a comprehensive network traffic analysis. The investigation involved a meticulous review of pcap files through Wireshark, a leading tool for network packet analysis. This process unveiled several files that were accessed or retrieved by the user, providing crucial insights for the cyber forensic examination.

In instances where file integrity and authenticity were critical, Hex Fiend was employed to scrutinize the raw hex data. This allowed for the recovery and reconstruction of files from the packet data, ensuring a precise and unaltered representation of the files as they were transmitted over the network.

The findings from this analysis have been compiled and presented in this report. It sheds light on the user's actions and the data exchanged during the session under investigation.


## Sub-task 1: Image Extraction from Network Traffic

- **Files Extracted:** `anz-logo.jpg`, `bank-card.jpg`
- **Extraction Process:**
  - I initiated the investigation by filtering the packet capture for HTTP traffic to isolate potential image transfers.
  - Browsing through the filtered results, I located the GET requests that initiated the download of the images.
  - After identifying the correct packets, I followed the TCP stream associated with each image download.
  - The TCP stream displayed data that appeared to be part of an image. To confirm this, I switched the view to ‘raw’ to analyze the data in hexadecimal format.
  - Within the hexadecimal data, I looked for the JPEG file signature starting with `FFD8` and ending with the footer `FFD9`.
  - Once the start and end of the image data were identified, I extracted the entire block of hex data between these two points.
  - Using the Hex Fiend, I pasted the copied hexadecimal data and saved the file with a `.jpg` extension, successfully retrieving the images that were accessed by the user.


## Sub-task 2: Additional Image Extraction

- **Files Extracted:** `ANZ1.jpg` and `ANZ2.jpg`
- **Extraction Method:**
  - The extraction process for additional images was consistent with the procedure outlined in Sub-task 1.
  - By observing the TCP stream for the respective GET requests, I pinpointed the hexadecimal data representing the image files.
  - With the identification of the hex data in the stream, I extracted the segments delineated by the JPEG file signature and footer (`FFD8` start and `FFD9` end).
  - This hexadecimal data was then copied and loaded into the Hex Fiend, where I proceeded to save the data as `.jpg` files to recover the images.
  - The difference in the network traffic for this images download I discovered was a hidden message in the data after
the end of the image.
The message said “You've found a hidden message in this file! Include it in your write up.” See screenshots `AZN1secret` and `AZN2secret`.

## Sub-task 3: Document Content Retrieval

- **File:** `how-to-commit-crimes.docx`
- **Content Discovery Approach:**
  - The initial step to uncovering the document's contents involved monitoring the TCP stream associated with the HTTP GET request for the document.
  - Within the raw representation of the stream, the textual content of the document was clearly visible, allowing for direct reading.
  
- **Document Message:**
  - The document `how-to-commit-crimes-message` carried a brief, yet concerning message laid out in a stepwise format:
    ```
    Step 1: Find target
    Step 2: Hack them
    ```
  - The nature of the message within the document hints at potentially malicious intent, marking it as suspicious.


## Sub-task 4: PDF File Extraction

- **Files:** `ANZ_Document.pdf`, `ANZ_Document2.pdf`, `evil.pdf`
- **PDF Retrieval Method:**
  - The procedure to access the content of the PDF files started by inspecting the TCP stream, a common step in these tasks.
  - The discovery of the PDF file signature was crucial, identified by the hexadecimal code `25504446`.
  - Observing the raw data indicated that the PDF content extended to the end of the TCP stream.

- **Extraction Process:**
  - Utilizing the hex data starting from the identified PDF signature, all the subsequent hex data was selected.
  - The entire selection was then transferred into the Hex Fiend.
  - Following this, the content was saved with a `.pdf` extension, successfully creating the PDF files.

- **Outcome:**
  - This extraction technique was effectively applied to all three PDF files in question, demonstrating its reliability.

## Sub-task 5: Decoding the Text File

- **File:** `hiddenmessage2.txt`
- **Observation:**
  - Initially, the file in question appeared to be a text file when viewed within the TCP stream.
  - However, it became evident that the file's content was not plain text, but encoded data.

- **Identification:**
  - On inspecting the data in hexadecimal format, it was recognized that the file bore the same signature as a JPEG image.

- **Extraction and Discovery:**
  - The hex data, starting from the JPEG signature, was extracted and transferred into Hex Fiend.
  - By saving the hex data as a JPEG, it was revealed that the text file `hiddenmessages.jpg` was, in fact, a disguised image.

## Sub-task 6: ATM Image Investigation

- **Files:** `atm-image.jpg` and `atm-image2.jpg`
- **Findings:** The corresponding TCP stream showed two sets of JPEG file signatures. This anomaly resulted in two different images being extracted from the same data stream, which may indicate hidden or additional data.

## Sub-task 7: Broken Image Retrieval

- **File:** `broken.png`
- **Recovery Process:** No PNG file signature was present in the TCP stream. Instead, the data was base64 encoded. After decoding using https://base64.guru/, a valid PNG image was obtained and analyzed for further information.

## Sub-task 8: Unveiling the Secure PDF

- **File:** `securepdf.pdf`
- **Initial Investigation:**
  - Upon analyzing the TCP stream associated with `securepdf.pdf`, it was discovered that the stream did not contain actual PDF data.

- **Discovery of Hidden Information:**
  - A hidden message was found at the end of the file stating: "Password is 'secure". The screenshot file is `PasswordIsSecure`.
  - The presence of a ZIP file signature was detected, which indicated that the downloaded file was mislabeled and was actually a ZIP archive.

- **Extraction Process:**
  - The hexadecimal data corresponding to the ZIP file was extracted using Hex Fiend and saved as a ZIP file.

- **Unzipping and Accessing the Contents:**
  - Upon opening the ZIP file, it was found to contain a PDF named `rawpdf.pdf`.
  - When attempting to open the `rawpdf.pdf`, a password prompt appeared.

- **Password Utilization and Content Revelation:**
  - The password 'secure', as found in the TCP stream, successfully unlocked the PDF.
  - The unlocked PDF contained the first two pages of a guide for internet banking. The file is `rawpdf.pdf`.



