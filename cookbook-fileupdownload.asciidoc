:toc: macro
toc::[]

= File download using org.apache.cxf

* 	Annotate download file method with @Produces("multipart/mixed").
* 		Read the file and convert it into byte array.
* Return MultipartBody (org.apache.cxf.jaxrs.ext.multipart.MultipartBody) as a response which contains byte array.

[source,java]
--------
@GET
@Path ("/download")
@Produces ("multipart/mixed")
   public MultipartBody getFiles() {

	//read object and store it into byte[]
	List<Attachment> atts = new LinkedList<Attachment>();
atts.add(new Attachment("obj", MediaType.APPLICATION_OCTET_STREAM, new ByteArrayInputStream(byte[])));
return new MultipartBody(atts, true);
}

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

* 	Annotate upload file method with @Consumes("multipart/mixed").
* 	Get list of attachments and process them to get input streams.
* Write these streams to the upload file server using file handling operations.
* 	Return acknowledgement of success if required.

[source,java]
--------
@POST 
@Consumes ("multipart/mixed") 
@Path ("/upload") 
public Response uploadFile( 
		// upload file to server                 
return null; 
}

--------


= Annotations @Consumes & @Produces

== @Consumes

* It specifies, the request is coming from the client. 
* You can specify the MIME (Multipurpose Internet Mail Extensions) type as @Consumes ("application/xml"), if the request is in xml format.
* The value of @Consumes is an array of String of acceptable MIME types
* For example:  @Consumes({"text/plain,text/html"})

== 	@Produces

* It specifies, the response is going to the client.
* You can specify the Mime type as @Produces ("application/xml"), if the response needs to be in xml format.
* The value of @Produces is an array of String of MIME types. 
* For example: @Produces({"image/jpeg,image/png"})

