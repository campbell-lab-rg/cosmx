# Export data from AtoMx to SFTP server
After logging into AtoMx, your home screen should contain the "Collections" corresponding to your projects. Each slide analyzed is called a "Study."
![[Pasted image 20241227055056.png]]

You can select your "Collection" by clicking on its title. The example for this case is "KC_4CosMx_S-24-2061." This will open you up to the list of corresponding "Studies" (slides).
![[Pasted image 20241227055252.png]]

Click on the slide with data you want to export (I clicked on "S-24-2051 Slide #3). This opens up the default home screen.
![[Pasted image 20241227055529.png]]

On a laptop, you can make the whole view fit into your screen by zooming out (Command + - on a Mac). Select the "Export" button to export the results. This will reveal the options on which components to export. 

*This is up to you.* Personally, I would recommend exporting the flat CSV files and the raw TIFF images. This gives you all of the expression information, segmentation images, and MxIF images that you might need for downstream analysis.

**Save the Output Export Access Information.** The Hostname and Username information will be needed in your terminal to move the exported data to your local computer. Be prepared to use the password that you use to log into AtoMx in the terminal.

![[Pasted image 20241227055820.png]]

When you hit "Export," the next step *will take time.* Depending upon the number of files/images, it will take longer. I usually just eep the window open as long as "Export in progress" is shown. They note that you should only export once.

![[Pasted image 20241227062821.png]]

Once the Export is complete, the "Export in progress" status will change to "Export successfully finished." Select "Show Export Details" for next steps.

![[Pasted image 20241227070311.png]]
The screen shows the information needed to obtain the Exported files from the SFTP location.
![[Pasted image 20241227070402.png]]
# Export data from SFTP server to local computer
Open your terminal. Note that these instructions are written for a Mac computer and have not been tested on a Windows computer. Navigate to the directory where you want to store the data. Be very careful at this step because you may risk overwriting files or sections.
```{bash}
$ cd tnowicki/cosmx/data/pilot/

$ pwd
/Users/katiecampbell/tnowicki/cosmx/data/pilot
```

Now you want to SSH into the SFTP server.
```{bash}
$ sftp KatieCampbell@mednet.ucla.edu@na.export.atomx.nanostring.com
KatieCampbell@mednet.ucla.edu@na.export.atomx.nanostring.com's password: 
Connected to na.export.atomx.nanostring.com.
sftp>
```

This shows that you are now within the server and can navigate the file structure. List the directories that are present.
```{bash}
sftp> pwd
Remote working directory: /
sftp> ls
S242051_Slide__1_26_12_2024_14_21_21_351                    S242051_Slide__3_27_12_2024_5_57_57_620                     
S242051_Slide__4_26_12_2024_14_13_27_947                    S242051_Slide__4_26_12_2024_20_16_48_491                    
```

This will list all of the directories of files that have been exported from AtomX. The directory `S242051_Slide__3_27_12_2024_5_57_57_620` corresponds to the files that we just exported from AtoMx (this is under the "Output Folder" in the Export details above). Change the directory to the folder containing the data of interest.
```{bash}
sftp> cd S242051_Slide__3_27_12_2024_5_57_57_620
sftp> pwd
Remote working directory: /S242051_Slide__3_27_12_2024_5_57_57_620
sftp> ls
RawFiles    flatFiles
```

To export the data to your local computer, run the following command:
```{bash}
sftp> get -R .
```

The `-R` option means "recursive," and it will copy all of the folders and subfolders, maintaining their structure.

This step will (again) take some time:
![[Pasted image 20241227072032.png]]

You will see the progress as files are copied to your local computer in your Finder as well. This is a good way to check whether the files are in the location you specified.

![[Pasted image 20241227072312.png]]
# Helpful reminders
* Make sure you know what directory you're downloading to, and that you are within that folder before you SSH into the SFTP server. This will ensure that the data is moved to the desired location.
* Be mindful of how much space you have on your computer. Each folder of raw files can be up to 80-100Mb. If you have an external hard drive, you can download directly to the folder by moving the the volume's directory in the terminal: `cd /Volumes/HardDriveName/folder/path`
