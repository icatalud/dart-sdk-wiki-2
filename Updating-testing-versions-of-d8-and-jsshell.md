# General
We pull jsshell and d8 from google cloud storage using the download_from_google_cloud_storage.py script from depot_tools. When updating you basically need to upload a new version (and leave the old one on cloud storage to not mess with our ability to do old builds)

IMPORTANT: DON'T EVER COMMIT BINARIES TO THE GIT REPO

# Updating d8
To ease the building of v8 binaries for mac and windows we have 3 bots on the fyi waterfall that can be forced to build a specific branch. On the bots, simply put the branch in the branch parameter when forcing a build. There is a step archiving the binaries to google cloud storage, which also contains a link to the files. Download these.

For arm, we don't have bots, so we build these ourselves locally.
```
fetch d8
git checkout #VERSION
```
You can check what specific #VERSION the bots did build.

Follow the v8 wiki on how to build for arm, in the last go I had to additionally do:
export CC (in addition to CXX and LINK)
comment out the building of tests in the build/all.gyp file
run make with werror=no

Now, inside the sdk checkout, go into third_party/d8
Copy in the d8 executable fetched from the bots to the linux, macos and windows directories, overwriting the existing binaries. Copy the build d8 arm binary to linux/d8-arm
Run (you need write access to the dart-dependencies bucket, ask for this if you feel you need it):
```
upload_to_google_storage.py -b dart-dependencies linux/d8
upload_to_google_storage.py -b dart-dependencies linux/d8-arm
upload_to_google_storage.py -b dart-dependencies macos/d8
upload_to_google_storage.py -b dart-dependencies windows/d8.exe
```
This will upload the files using a calculated hash of the content. It will also update the .sha files that we have checked into the repo (i.e., linux/d8.sha, linux/d8-arm.sha, macos/d8.sha and windows/d8.exe.sha)

Change the description in the README.google file (updating the version and the date in most cases)

Upload a cl for review, it should contain the 4 sha files and the README.google file - but no binaries, remember:

IMPORTANT: DON'T EVER COMMIT BINARIES TO THE GIT REPO (YES WE STATED THIS BEFORE AS WELL)

Land the cl, everybody will now pull down the new versions.

# Updating jsshell
TODO(ricow): move to cloud storage