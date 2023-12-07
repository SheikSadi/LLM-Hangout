## Building and Saving a Docker Image on Ubuntu 22.04 with Docker Desktop
Share: https://g.co/bard/share/f1b4dcd502cd

Here's how to build a Docker image and copy it to an external disk on Ubuntu 22.04 using Docker Desktop:

**1. Build the Docker Image:**

* Open a terminal window.
* Navigate to the directory containing your Dockerfile.
* Run the following command to build the image:

```
docker build -t <image-name>:<tag> .
```

* Replace `<image-name>` with the desired name for your image.
* Replace `<tag>` with the desired tag for the image (optional).
* If the build process is successful, you will see a message indicating that the image was built successfully.

**2. Save the Docker Image to an External Disk:**

Here are two ways to save the Docker image to an external disk:

**Option 1: Using `docker save`**:

* Connect your external disk to your computer.
* Make sure the disk is mounted and accessible.
* Run the following command to save the image to a file on the external disk:

```
docker save -o <output-file.tar> <image-name>:<tag>
```

* Replace `<output-file.tar>` with the desired filename for the saved image file on the external disk.
* Replace `<image-name>` and `<tag>` with the name and tag of the image you built.
* The `docker save` command will create a TAR archive containing the image layers.

**Option 2: Using `docker export`**:

* Run the following command to export the image as a set of TAR archives, one for each layer:

```
docker export <image-name>:<tag> | tar -C <output-directory> -tf -
```

* Replace `<image-name>` and `<tag>` with the name and tag of the image you built.
* Replace `<output-directory>` with the directory on the external disk where you want to save the exported image layers.
* This command will export each layer of the image as a separate TAR archive to the specified directory.

**3. Copy the Saved Image File(s) to the External Disk:**

* If you used `docker save`, simply copy the `<output-file.tar>` file to the desired location on your external disk.
* If you used `docker export`, you need to copy all the individual TAR archives from the `<output-directory>` to the desired location on your external disk.

**4. Import the Image (Optional):**

* Once you have saved the image to your external disk, you can import it back into Docker on another machine using the following command:

```
docker load -i <input-file.tar>
```

* Replace `<input-file.tar>` with the filename of the saved image file on the external disk.
* This command will import the image layers from the TAR archive and make the image available for use on the new machine.

**Additional Notes:**

* Make sure you have sufficient space on your external disk to save the image file(s).
* You can use any of the available tools to manage and copy files to your external disk, such as `cp` or `rsync`.
* If you encounter any errors during the build or save process, refer to the Docker documentation or seek online help for troubleshooting.

**Additional Resources:**

* Docker documentation on Building Images: [https://phoenixnap.com/kb/create-docker-images-with-dockerfile](https://phoenixnap.com/kb/create-docker-images-with-dockerfile)
* Docker documentation on Saving and Loading Images: [https://docs.docker.com/engine/reference/commandline/save/](https://docs.docker.com/engine/reference/commandline/save/)
