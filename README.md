

# Asp.net-core-React-File-Upload

 - React File Upload  
 - Post Image/File From React Form
 - Upload / Save Image in Web API

## React Code Sample 

```
import React, { useState } from "react";
import axios from "axios";

export const FileUpload = () => {
  const [file, setFile] = useState();
  const [fileName, setFileName] = useState();

  const saveFile = (e) => {
    console.log(e.target.files[0]);
    setFile(e.target.files[0]);
    setFileName(e.target.files[0].name);
  };

  const uploadFile = async (e) => {
    console.log(file);
    const formData = new FormData();
    formData.append("formFile", file);
    formData.append("fileName", fileName);
    try {
      const res = await axios.post("https://localhost:44323/api/Uploadfile", formData);
      console.log(res);
    } catch (ex) {
      console.log(ex);
    }
  };

  return (
    <>
      <input type="file" onChange={saveFile} />
      <input type="button" value="upload" onClick={uploadFile} />
    </>
  );
};

```

	
	
## Web API Model

```
 public class FileModel
    {
        public string FileName { get; set; }
        public IFormFile FormFile { get; set; }
    }
```

## Web API Controller Code Sample 

```
    [Route("api/Uploadfile")]
    [ApiController]
    public class UploadfileController : ControllerBase
    {

        [HttpPost]
        public ActionResult Post([FromForm] FileModel file)
        {
            try
            {
                string path = Path.Combine(Directory.GetCurrentDirectory(), "wwwroot", file.FileName);

                using (Stream stream = new FileStream(path, FileMode.Create))
                {
                    file.FormFile.CopyTo(stream);
                }

                return StatusCode(StatusCodes.Status201Created);
            }
            catch (Exception)
            {
                return StatusCode(StatusCodes.Status500InternalServerError);
            }
        }

    }

```
## Db Context For Asp.net core Database First

```
 public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
        {
        }
        public DbSet<Categories> Categories  { get; set; }
       

       

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Categories>().ToTable("Categories");
        }
    }
    
  ```

 ## Output


<img src="https://i.stack.imgur.com/2eLCr.gif" alt="user avatar" width="750" height="450" class="bar-sm bar-md d-block">  

I hope it would help to upload files using React and Asp.net core. Thanks

