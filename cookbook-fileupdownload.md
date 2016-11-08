:toc: macro
toc::[]

= File download using org.apache.cxf

== Generic solution - code snippet

MIME (MIME (Multipurpose Internet Mail Extensions) “application/octet-stream” is used as default MIME type for file download. 

We define a regular interface to define the API of the service and annotate it with JAX-WS annotations:

[source,java]
--------
@SuppressWarnings("javadoc")
  	@Produces("application/octet-stream")
  	@GET
  	@Path("/product/downloadFile")
  	public Response getDownloadableFile() throws SQLException, IOException;
--------

And here is a simple implementation of the service:
[source,java]
--------
@Override
public Response  getDownloadFile() throws SQLException, IOException {
File file = new File("D:/Users/…/abc.pdf"); // path for .pdf or //.xlsx file 
    	ResponseBuilder response = Response.ok((Object) file);
response.header("Content-Disposition", "attachment; filename=test.xlsx"); // file name as response .pdf or .xlsx
    	return response.build();

  }

--------

= File upload using org.apache.cxf



...............................................................................


Devon workflow package – File Upload & Download

ABSTRACT 
This document specifies the possible solutions for the functionality “File Upload and Download” from org.apache.cxf support.
INTRODUCTION
This technical document describes:
	OASP4J functionality that supports file upload and download.
	Solutions for file upload/download from org.apache.cxf.

	OASP4J existing functionalities which supports file upload and download
OASP4J provides REST service to upload/download file with Content-type multipart/mixed. This functionality is provided by offermanagement module. Code Snippet of the same follow:  
•	File Download 
Service Definition
package io.oasp.gastronomy.restaurant.offermanagement.service.api.rest;
public interface OffermanagementRestService

@SuppressWarnings("javadoc")
@Produces("multipart/mixed")
@GET
@Path("/product/{id}/picture")
public MultipartBody getProductPicture(@PathParam("id") long productId) throws SQLException, IOException;
				
		

Service implementation
package io.oasp.gastronomy.restaurant.offermanagement.service.impl.rest;
public class OffermanagementRestServiceImpl

@Override
  public MultipartBody getProductPicture(long productId) throws SQLException, IOException {

    Blob blob = this.offermanagement.findProductPictureBlob(productId);
    // REVIEW arturk88 (hohwille) we need to find another way to stream the blob without loading into heap.
    // https://github.com/oasp/oasp4j-sample/pull/45
    byte[] data = IOUtils.readBytesFromStream(blob.getBinaryStream());

    List<Attachment> atts = new LinkedList<>();
    atts.add(new Attachment("binaryObjectEto", MediaType.APPLICATION_JSON,
        this.offermanagement.findProductPicture(productId)));
    atts.add(new Attachment("blob", MediaType.APPLICATION_OCTET_STREAM, new ByteArrayInputStream(data)));
    return new MultipartBody(atts, true);

  }

•	File Upload
Service Definition
package io.oasp.gastronomy.restaurant.offermanagement.service.api.rest;
public interface OffermanagementRestService
@SuppressWarnings("javadoc")
  @Consumes("multipart/mixed")
  @POST
  @Path("/product/{id}/picture")
  public void updateProductPicture(@PathParam("id") long productId,
      @Multipart(value = "binaryObjectEto", type = MediaType.APPLICATION_JSON) BinaryObjectEto binaryObjectEto,
      @Multipart(value = "blob", type = MediaType.APPLICATION_OCTET_STREAM) InputStream picture)
          throws SerialException, SQLException, IOException;

Service implementation
package io.oasp.gastronomy.restaurant.offermanagement.service.impl.rest;
public class OffermanagementRestServiceImpl
@Override
  public void updateProductPicture(long productId, BinaryObjectEto binaryObjectEto, InputStream picture)
      throws SerialException, SQLException, IOException {

    Blob blob = new SerialBlob(IOUtils.readBytesFromStream(picture));
    this.offermanagement.updateProductPicture(productId, blob, binaryObjectEto);

  }





•	How to call getProductPicture REST service
 
To call REST service getProductPicture, hit the service Url http://localhost:8081/services/rest/offermanagement/v1/product/1/picture . In the service URL, 1 is the product Id.

 

Re-use file upload/download functionality in OASP4J

OASP4j functionality for upload/download can be re-used with modifications to make the functionality generic such that the existing functionality is not broken and can be used any new service.

Blob objects are being used as part of the upload/download file functionality in OASP4j. This is an overhead since Blob objects consume large footprints in Heap Memory. Alternative solution has to be found to address this issue.





 
	Solutions for file upload/download from org.apache.cxf

	File Download

	Annotate download file method with @Produces("multipart/mixed")
	Read the file and convert it into byte array. 
	Return MultipartBody (org.apache.cxf.jaxrs.ext.multipart.MultipartBody) as a response which contains byte array.

Code snippet

@GET
@Path ("/download")
@Produces ("multipart/mixed")
   public MultipartBody getFiles() {

	//read object and store it into byte[]
      	List<Attachment> atts = new LinkedList<Attachment>();
atts.add(new Attachment("obj", MediaType.APPLICATION_OCTET_STREAM, new ByteArrayInputStream(byte[])));
        return new MultipartBody(atts, true);
   }

	File Upload 
 
	Annotate upload file method with @Consumes("multipart/mixed")
	Get list of attachments and process them to get input streams.
	Write these streams to the upload file server using file handling operations.
	Return acknowledgement of success if required.

Code snippet


	@POST 
        @Consumes ("multipart/mixed") 
        @Path ("/upload") 
        public Response uploadFile( 
		// upload file to server                 
                return null; 
        }


	File download Test case 
This test case has been tested for downloading “.pdf” file from given file location. Below code snippets can be used for expected result.

RestService.java 

	@SuppressWarnings("javadoc")
 	@Produces("application/pdf") // type of file to be produced.
 	@GET
 	@Path("/product/downloadFile") //path for webservice
 	public Response getDownloadFile() throws SQLException, IOException;

RestServiceImpl.java 

	@Override
public Response getDownloadFile () throws SQLException, IOException                                                                                                                           {

File file = new File ("D:/Users/.../abc.pdf"); // pdf file path, for //test case its hard coaded, can be picked from anywhere
    	ResponseBuilder response = Response.ok((Object) file);
response.header("Content-Disposition", "attachment; filename=test.pdf");
    	return response.build();


  }















Test case result
 

 
 As depicted above, .pdf file has been downloaded successfully. 


Note:
The documents with the different extensions (“.doc”, “.txt” etc) can be downloaded with the help of org.apache.cxf. Please go through references. These references also contain the information about the file upload feature. 











	Generic code test case 
This test case has been tested for downloading “.pdf” & “.xlsx” files from given file location. 
MIME (MIME (Multipurpose Internet Mail Extensions) “application/octet-stream” is used as default MIME type for file download. 

Below code snippets can be used for expected result.

RestService.java

	@SuppressWarnings("javadoc")
  	@Produces("application/octet-stream")
  	@GET
  	@Path("/product/downloadFile")
  	public Response getDownloadFile() throws SQLException, IOException;

RestServiceImpl.java

	@Override
public Response  getDownloadFile() throws SQLException, IOException {
File file = new File("D:/Users/…/abc.xlsx/pdf"); // path for .pdf or //.xlsx file 
    	ResponseBuilder response = Response.ok((Object) file);
response.header("Content-Disposition", "attachment; filename=test.xlsx"); // file name as response .pdf or .xlsx
    	return response.build();

  }













Generic code  test case result 
For .pdf file 

 
For .xlsx file 

 


Annotations @Consumes & @Produces
	@Consumes
o	It specifies, the request is coming from the client. 
o	You can specify the MIME (Multipurpose Internet Mail Extensions) type as @Consumes ("application/xml"), if the request is in xml format.
o	The value of @Consumes is an array of String of acceptable MIME types
	For example:  @Consumes({"text/plain,text/html"})


	@Produces
o	It specifies, the response is going to the client.
o	You can specify the Mime type as @Produces ("application/xml"), if the response needs to be in xml format.
o	The value of @Produces is an array of String of MIME types. 
	For example: @Produces({"image/jpeg,image/png"})	

For more info visit https://docs.oracle.com/cd/E19798-01/821-1841/gipzh/index.html 
For MIME list visit : https://www.sitepoint.com/web-foundations/mime-types-complete-list/ 
















References

http://cxf.apache.org/docs/jax-rs-multiparts.html
http://www.ibm.com/developerworks/library/ws-devaxis2part3/
http://www.benchresources.net/apache-cxf-jax-rs-restful-web-service-for-uploadingdownloading-text-file-java-client/
http://aruld.info/handling-multiparts-in-restful-applications-using-cxf/
http://www.mattorama.net/blog/2011/2/19/file-uploads-with-cxf-multipart-form-posts.html

Download PDF file using CXF
Download DOC file using CXF
Download EXCEL file using CXF
Download IMAGE file using CXF
Download TEXT file using CXF








