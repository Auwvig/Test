function afterFormSubmit(e) {
  const info = e.namedValues; // Form data passed here
  createPDF(info); // Create PDF
}

function createPDF(info) {
  try {
    Logger.log("Starting PDF creation...");

    // Retrieve folders and template document by their IDs
    const pdfFolder = DriveApp.getFolderById("1vefqHEE2vMkJQfJ91LUFUR0SQnMA0dkB");
    const tempFolder = DriveApp.getFolderById("1dcvOpZEL6a75OIoOn0dTtj59cdlIJyx1");
    const templateDoc = DriveApp.getFileById("1mMCzCX58MGkfro-hmwXq6nCxKw33OrJ3eIUEG7_p0iY");

    Logger.log("Folders and template document retrieved successfully.");

    // Make a copy of the template document in the temp folder
    const newTempFile = templateDoc.makeCopy(tempFolder);
    Logger.log("Template copied to temp folder: " + newTempFile.getName());

    // Open the new copy and get its body for editing
    const openDoc = DocumentApp.openById(newTempFile.getId());
    const body = openDoc.getBody();

    // Replace placeholders with actual data from the form submission
    body.replaceText("{fn}", info['First Name'][0] || "");
    body.replaceText("{ln}", info['Last Name'][0] || "");
    body.replaceText("{addr}", info['Shipping Address'][0] || "");
    body.replaceText("{qty}", info['Quantity Required'][0] || "");
    openDoc.saveAndClose();
    Logger.log("Placeholders replaced successfully in the document.");

    // Convert the updated document to PDF format
    const blobPDF = newTempFile.getAs(MimeType.PDF);
    const pdfFileName = `${info['First Name'][0]} ${info['Last Name'][0]}`;
    const pdfFile = pdfFolder.createFile(blobPDF).setName(pdfFileName);
    Logger.log("PDF created in pdfFolder: " + pdfFile.getName());

    // Log the PDF URL to check the file is accessible
    Logger.log("PDF URL: " + pdfFile.getUrl());

    // Remove the temporary file from the temp folder to clean up
    tempFolder.removeFile(newTempFile);
    Logger.log("Temporary file removed from temp folder.");

    return pdfFile; // Optionally return the file for further use

  } catch (error) {
    Logger.log("Error in createPDF: " + error.toString());
  }
}

// Testing function - can be run in Apps Script editor
function testCreatePDF() {
  const testInfo = {
    'First Name': ["John"],
    'Last Name': ["Doe"],
    'Shipping Address': ["123 Test St"],
    'Quantity Required': ["5"]
  };
  createPDF(testInfo);
}

