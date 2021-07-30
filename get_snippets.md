# Getting things

By getting, I mean download data, downloading toolboxes, linking datasets. Most of the time this can be done with a shell script or a python script.

#### Contents


## Local copy

Perhaps the simplest operation is to copy a file or folder from one place on a computer to another e.g.

    cp ~/projects/some_folder/data/internal/dataset.nc destination/
    
or recursively copy a directory with `-r`

    cp -r ~/projects/some_folder/data/internal/data_dir destination/
    
or use wildcards to copy files that match a pattern

    cp ~/projects/some_folder/data/internal/data_dir/file_*.mat destination/
    
One of many downsides of copy is that it uses up more memory, and generally it is not recommended or necessary. 
    
## Local hard links

Better than copying is to create a link. This works within a filesystem but not between different filesystems. 

    ln ~/projects/some_folder/data/internal/dataset.nc destination/dataset.nc
    
or

    ln ~/projects/some_folder/data/internal/data_dir destination/data_dir
    
which is exactly equivalent to the original file but uses no more memory. If the original file is moved a hard link will still work. If you delete a hard link it will not delete the original. 

## Local symbolic links

Symbolic links are like signposts to a file and are created with the `-s` option. They work both within and between filesystems. 

    ln -s ~/projects/some_folder/data/internal/dataset.nc external/dataset.nc
    
or

    ln -s ~/projects/some_folder/data/internal/data_dir external/data_dir
    
or

    ln -s /Volumes/mounted_drive/dataset.nc external
    
If the original file is moved it will break the link. If the original is changed, the link will still work. Deleting the link does not delete the original.

## rsync - if you have to copy use this

`rsync` is a utility that synchronises files. If a file already exists in the destination and the source is unmodified, it will not bother transferring any data which saves time and bandwidth. If a source file has changed, the destination will be updated in an efficient way. Use like:

    rsync ~/projects/some_folder/data/internal/dataset.nc destination/
    
or recursively sync a directory with `-r`

    rsync -r ~/projects/some_folder/data/internal/data_dir destination/
    
or use wildcards

    rsync ~/projects/some_folder/data/internal/data_dir/file_*.mat destination/
    
## Remote copy with `scp` and `rsync`

If you need to copy files from a remote server that you normally access via `ssh`, use `scp`, e.g.

    scp usename@host:some_dir/dataset.nc destination/
    
it uses nearly the same syntax as `cp`, so for folders, 

    scp -r usename@host:some_dir/a_folder destination/
    
Or better yet, `rsync` works remotely too:

    rsync -rv username@host:some_dir/a_folder destination/a_folder
    
where `-v` specifies verbose output, or more information about what is going on.

## Mounting with `sshfs`

If you have very fast access to a remote machine (say on a local area network), it might be worth mounting the file system using `sshfs` and then symbolically linking to the files. Every time you access a file it will transfer data over the network. This may not be desirable if your network is slow. Usage is something like this:

    sshfs [user@]host:[remote_directory] mountpoint [options]
    
Unmount with

    umount mountpoint
 
## Mounting an smb shared drive

This can be done graphically on macOS using finder by clicking 'Go' and then 'Connect to Server'. Alternatively use the `mount_smbfs` command. The same point about network speed mentioned above for `sshfs` matters here too.

    mount_smbfs //user@SERVER/folder /Volumes/folder
    
Unmount with

    umount /Volumes/folder

## Accessing cloud storage with [`rclone`](https://rclone.org/)

Often datasets are stored in the cloud with dropbox, google drive, box or some similar tool. Everyone knows about the graphical interfaces for these software, which usually look like a folder on your computer. The `rclone` utility provides command line access to such storage although the setup is a little laborious - you have to generate an access token on every system where you use `rclone`.

Setup with,

    rclone config
    
and then use like

    rclone copy source:sourcepath folder/on/your/computer
    
where `source` is the name you gave your cloud storage during the configuration.

Or even better synchronise!

    rclone sync source:sourcepath folder/on/your/computer

Here is an example of a shell script that I wrote for a project. It was contained in a file called `get_vmp_pelican_2021.sh`. It allows the user to input a drive name, but if too much time passes, it tries a default name. 

    echo "Input rclone drive name:"

    read -t 10 drive

    if [ -z "$drive" ]
    then
        drive=drive-GOM
        echo "Too slow. Using default: $drive"
    fi

    rclone sync -P $drive:data/Pelican2021/data/vmp/RAW_P_file ./Pelican2021/vmp/P_files/

## Downloading with `wget`

To install the matlab Gibbs Seawater Toolbox I use:

    wget http://www.teos-10.org/software/gsw_matlab_v3_06_12.zip
    unzip gsw_matlab_v3_06_12.zip -d gsw_matlab_v3_06_12
    
## Downloading with `git`

To install matlab BrewerMap colormaps I use:

    git clone --depth=1 --branch=master https://github.com/DrosteEffect/BrewerMap BrewerMap
    rm -rf ./BrewerMap/.git