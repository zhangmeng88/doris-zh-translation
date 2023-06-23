# DORIS User Interface (Desktop Version for batch processing) 

DORIS User Interface is a desktop software that can be installed on local computers. It is designed to allow effortless batch processing of large volumes of death certificates. Whether working with text or code modes, this software analyzes thousands of death certificates and supports multiple formats, Excel, CSV, and JSON.

DORIS's analysis capabilities play a crucial role in extracting valuable insights from the vast amount of data contained in death certificates. It employs an advanced algorithm to process and interpret the information, enabling users to uncover patterns, trends, and underlying causes of death. This analysis is invaluable for public health surveillance, epidemiological studies, and policy decision-making, as it facilitates the identification of key health indicators, disease prevalence, and potential areas for intervention.

DORIS's ability to handle thousands of death certificates in text and code mode and support multiple file formats offers immense benefits to users. It streamlines data analysis processes, promotes interoperability with various data sources, and empowers users to gain meaningful insights from extensive datasets, ultimately contributing to improved public health outcomes and informed decision-making.

![DORIShomepagepicture ](img/DORIShomepage.png)

## Download and Installation

1. Go to the DORIS homepage by visiting the website icd.who.int/doris.
2. On the homepage, locate and click on the "More information and Download" option. This will redirect users to the page containing instructions for using the DORIS Offline Tool.
3. Under for the section titled "Download and Installation" users will find the necessary information and resources to download the DORIS tool. The DORIS tool for batch processing can be downloaded from: [Desktop Version](https://icdcdn.who.int/doris/DorisUI_0.6.0.0_x64-rc1.msix)
4. Follow the provided instructions to download the DORIS tool onto local desktops or computer systems. The installation process involves double-clicking on the downloaded file and following the on-screen instructions provided. 

By following these steps, users will be able to successfully download the DORIS User Interface Desktop Version and initiate batch processing activities.

![dorisuipicture](img/dorisui.png)

## Supported input files - Text and code mode.

The DORIS tool can handle two distinct input modes: text and ICD-11 code. This versatility allows users to choose between entering the cause of death information in a text format or utilizing pre-coded ICD-11 classifications for efficient processing. By accommodating both input modes, the tool caters to different user preferences and data availability in countries.

The tool's processing capabilities are highly efficient. It can handle a large volume of death certificates, enabling the processing of thousands or even millions of certificates in just a matter of minutes. This processing speed ensures that users can swiftly analyze and extract valuable insights from a significant number of certificates without experiencing significant delays or performance issues. 

The tool's flexibility in handling different input modes and its processing speed make it a reliable and efficient solution for analyzing and extracting meaningful information from death certificates. Whether dealing with text-based or coded certificates, users can rely on the tool to swiftly process large quantities of data, assisting them to make informed decisions in a timely manner.

![textcodemodessupportedpicture ](img/textcodemodessupported.png)

## Supported file formats - Excel, CSV, and JSON.

By supporting multiple file formats Excel, CSV, and JSON, DORIS tool ensures compatibility and ease of use for users working with different data sources. On the DORIS homepage, users can find the template for the input files that are supported by the DORIS tool. These template are specifically designed to ensure compatibility and proper data input in the DORIS tool. By accessing the DORIS homepage, users can easily download the template and utilize it as a guideline for preparing their input file, ensuring that it meets the required format and structure. 

**Excel files** are widely used for data management and analysis, while **CSV** (Comma-Separated Values) and **JSON** (JavaScript Object Notation) formats offer flexibility in exchanging and storing structured data. DORIS's compatibility with these formats enables users to import and analyze death certificates without the need for extensive data conversion or manipulation.

![fileformatsupportedpagepicture ](img/fileformatsupported.png)

## Using DORIS User Interface Desktop Version - Step-by-Step Instructions

In order to use the DORIS tool effectively, users are required to provide an input file that includes the death certificates they want to process and analyze. The tool will examine the information on the death certificates and f the ICD mortality rulebase, which operates in the background, to determine the underlying cause of death. By ensuring that the input file adheres to the required format, the tool can accurately analyze and process the information from the death certificates, providing users with an automatic determination of the underlying cause of death.

Start by selecting the specific death certificate format (CSV or JSON) that you require. Click on the format you need and proceed to download it onto your computer.

Once the download is complete, open the downloaded file. It is important to fill in the cause of death information accurately, ensuring that you do not modify the format or change the order of the columns. Maintain the original structure of the file.

Launch the DORIS tool on your desktop. Look for the option to load a file for processing and select the file you just filled in with the cause of death information.

After selecting the file, click on the processing button to initiate the file processing procedure. The tool will begin processing the file, and once it completes the task, the output file will be automatically saved in the same location as the input file.

By following these steps, you can effectively utilize the DORIS User Interface Desktop Version to run and process death certificate files, ensuring that the cause of death information is accurately captured and maintained.

It supports the following file  formats:

### JSON format
- The JSON format is based on the standard [Death Certificate Exchange Format](json-format.md) 

### Tabular format (Excel and CSV)
- The Excel and CSV formats are tabular representation of the certificate and output of DORIS. The format specification and sample files can be found 
here: [Tabular Format Specification](csv-excel-format.md)

### Loading the file and processing

TBA

### Checking the output file

TBA

![DORIS UI Screenshot](img/dorisuiscreen.png)


