# DORIS Desktop User Guide
DORIS Desktop requires a `DORIS Dataset` to work with. You may import certificates from a file in one of the supported formats. Once a file is imported, it is saved as a `DORIS Dataset`. After that, everything done in the software uses this `DORIS dataset`. For example, If you use `Process` to compute the underlying cause of death on all certificates, This information is saved into the Doris Dataset and remembered.
You can always open a DORIS Dataset using `Open DORIS Dataset` menu to return to where you left off. 

You can always `Export` the certificates in a `DORIS Dataset` into supported formats.

## Importing Data
You may import certificates using one of the supported file formats. Details of the formats and sample files are available at the links below:

- Detailed description of the [Tabular format for Excel and CSV files](csv-excel-format.md)

- Detailed description of the [Standard JSON format](json-format.md)

![Import menu](img/menu-import.png){: style="width:50%"}

To import from a file, choose `Import from .xlsx, .csv or .json Files` menu item under the `Data` menu. After that, you need to pick the file that you'd like to import. Then the tool will ask you for a file name for the `DORIS dataset` that will be created. By default, the Doris datasets are created under the Documents folder in Windows. 

![Dataset name dialog](img/dataset-filename.png){: style="width:50%"}

After giving a file name and clicking `OK`, DORIS will import the file and will show you the contents. The system will show a progress bar at the bottom of the screen if it's a large file that takes time to import.

## Working with DORIS Dataset
DORIS Desktop works with DORIS datasets which are created after you import data using a supported file format. By default, the Doris datasets are created under the Documents folder in Windows. This can be changed from the `Settings`/`Change Default Dataset Folder` menu.


### Opening a Dataset
You could open an existing DORIS dataset using the `Data`/`Open Doris Dataset` menu

![Open Dataset](img/dataset-open.png){: style="width:50%"}

### Dataset Information
Once a dataset is open, you may use the Dataset Information button or `Data`/`Dataset Information` menu to see information on the dataset.
![Dataset Information](img/dataset-info.png){: style="width:50%"}

This information contains the number of certificates, whether the data contains coded or textual data, whether the data is processed, and various other information on the dataset. You may see a sample below

![Dataset Information2](img/dataset-info2.png){: style="width:80%"}


### Working with certificates that use textual data
If the imported data does not contain codes but textual conditions, then the tool will add codes using text-to-code processing during the importing phase. This process places the auto-assigned codes in the `Code(auto)` column in the tool together with an additional column `Match` that shows the quality of the text-to-code matching process. We suggest all matching levels except for `GoodMatch` need to be manually checked by experts.
![Text to code columns](img/texttocode-columns.png){: style="width:80%"}

Text-to-code conversion issues button can be used to easily filter out the cases that contain text-to-code processing with non-GoodMatch results.
![Text to code issues button](img/texttocode-issues-button.png){: style="width:50%"}

### Processing the Dataset (Underlying Cause of Death Detection)

Processing the file will compute the underlying cause of death for each certificate. This is done by using the menu `Process`
![Process menu](img/process-menu.png){: style="width:50%"}

If it's a large dataset, the system will display a progress bar at the bottom of the screen to show the progress.

Once the processing is finished, the results are in the `Underlying Cause of Death (UCOD)` column. If the underlying cause of death is a postcoordination combination it is placed in the `UCOD with postcoordination information` column. Rejected certificates are marked in the `Reject` column and Errors and Warnings are placed in the `Error` and `Warnings` columns.

![UCOD columns](img/ucod-columns.png){: style="width:80%"}

 Certificates that are rejected or have other problems can be easily filtered out by using the Processing Issues button.
![Processing issues button](img/processing-issues-button.png){: style="width:40%"}

### Filtering and Sorting
It is possible to sort the certificates by the values of a selected column by clicking on the column label. 

Similarly, filtering is possible by using the filter icons located near the column labels.
![Filtering](img/filter.png){: style="width:50%"}

### Editing Individual Certificates

Clicking the number in the `Id` column opens the certificate in full-view mode.
![Open certificate](img/open-cert.png){: style="width:80px"}

In full-view mode, all information in the certificate as well as the computed underlying cause of death is shown to the user
![Certificate full-view](img/cert-fullview.png){: style="width:80%"}

When opened, the system will not allow editing the certificate. To edit it you need to click the unlock button at the bottom.
![unlock icon](img/unlock.png){: style="width:30px"}

Once unlocked, you can edit the certificate. Condition lines accept only valid ICD-11 Codes. Editing textual diagnosis is not possible however you could overwrite the textual diagnosis by providing code(s)

Once editing is complete, you may save it by clicking the `Save` button, If not saved, the tool will ignore the changes made after closing the full-view.

Closing full-view mode is done by clicking the `X` at the top left corner.

`Process` button in the full-view will reprocess the certificate to find the underlying cause of death. This is saved only after `Save` is pressed. 

It is also possible to process all edited certificates by using the `Process` menu after closing the full-view.


## Settings
### Changing Language

### Changing ICD-11 Version

### Changing default Doris dataset folder

## Exporting Data


