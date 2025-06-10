# Release Notes for Doris Desktop

## Version 1.1
- Doris rules version 1.1 is implemented.
    - **Enhanced Visualization Features**  
          In addition to the existing **textual output**, the following **three new views** have been introduced:
              - **Tabular Report**: An interactive view that shows UCOD selection steps in a table format. Users can follow the steps sequentially, with corresponding rules highlighted on the certificate.
              - **Rule Flow Report**: Displays a visual sequence of the applied rules leading to the selected UCOD.
              - **Rule Sequence Report**: Presents a horizontal timeline of applied rules, listing them in the order they were triggered.
    - **Improved Algorithm Accuracy**  
      This release includes enhancements to the DORIS algorithm, improving accuracy and consistency in cause-of-death coding and reporting.
              - Refined specificity for infectious diseases, neoplasms, and injury-related conditions.
              - Improved logic for HIV, tuberculosis, and substance intoxications.
              - Better handling of external causes and injury-related deaths.
     - **macOS version now available**  
      Broadened compatibility for users operating on Apple systems.
       

## Version 1.0 (Initial version)

-	Textual certificate support added. It can efficiently process a substantial volume of data, handling thousands of text-filled certificates in just few minutes. With this update, users can expect fast results, including corresponding ICD-11 codes, entity titles, and the computed UCOD.
-	Significant efforts were dedicated to enhance the user interface of the web version. The improvements aim to provide a more intuitive and user-friendly experience, allowing users to navigate through individual certificates with ease and save the processed certificates on local computers. 
-	The analysis capabilities of DORIS were expanded by incorporating fields from frame B, such as manner of death, pregnancy and other. By considering these additional fields, DORIS can now perform more comprehensive and accurate analyses, resulting in higher-quality UCOD selection.
-	Additional control checks and warnings were introduced. These new features serve to increase the accuracy and reliability of the results.
-	AN ANACOD-3 compatible output was introduced as an additional output option from DORIS. This update ensures easier integration with other systems and workflows, enhancing the overall efficiency of data processes.
-	The digital mortality rule base has undergone substantial improvements and clarifications based on the invaluable feedback received during MRD workshops. These enhancements further optimize the performance and reliability of DORIS.
-	The DORIS  online user guide is now available.
-   The rules that are used by the tool are now maintained by using our Mortality Rules Authoring platform.
