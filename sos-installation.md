## Install SoS workflow and notebook in MacOS

1. Make sure you have python 3.6 or more installed
2. Run the following commmands for SoS-workflow (worked for me)
    ```
    pip3.7 install sos
    pip3.7 install sos-pbs
    ```
3. Run the following commands for SoS-notebook
    ```
    pip3.7 install sos-notebook
    pip3.7 install sos-papermill
    pip3.7 install sos-r
    
    ```
 4. In MacOs the sos and sos-runner will be installed in a folder called 
 
    /Library/Frameworks/Python.framework/Versions/3.7/bin/sos
    
 5. You will have to create a symbolic link where the miniconda3 is installed
 ```
    cd /User/.../miniconda3/condabin/
    ln -s /Library/Frameworks/Python.framework/Versions/3.7/bin/sos sos
    ln -s /Library/Frameworks/Python.framework/Versions/3.7/bin/sos-runner sos-runner
    
  ```

5. Now you will be able to run the statgen-setup script to acces de VM
