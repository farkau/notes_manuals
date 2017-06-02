***********************
Useful Command Overview
***********************

Here I just collect some notes I took across the years to easily share 
with myself and others from the PSY interested in using it.
It is divided into basic topics but mostly I just search it by keywords.

=======
Network
=======

Cluster
-------

*vnc access*
    .. code-block:: bash
    
        vnc -via yourAcc@CLUSTER.de :2

*kill and start a new vnc session (and change resolution)*
    .. code-block:: bash

        # 1. kill
        ssh yourAcc@CLUSTER.de
        vncserver -kill :2

        # 2. start a new one
        vncserver :2 #(new displaynumber)
        
        # 3. change the resolution
        vncserver -geometry 1900x1100 :2

*connect to cluster*
    .. code-block:: bash
        
        ssh yourAcc@CLUSTER.de

*run local editor on cluster*
    .. code-block:: bash
        
        ssh -X yourAcc@CLUSTER.de
        gedit

*link to local folders*
    .. code-block:: bash
        
        mkdir yourFolder/
        sshfs -o idmap=user -o transform_symlinks -o follow_symlinks yourAcc@CLUSTER.de: yourFolder/ 

*copy stuff inside cluster*
    .. code-block:: bash
        
        cp from to
        
        Don‘t forget to chmod that others can copy
        chmod 644 copiedfile

*sync folders with cluster (and local)*
    .. code-block:: bash
        
        rsync -rv --bwlimit=70000 XFoldertosyncX CLUSTER:/home/yourAcc/...
            -rv 				#keep rights
            --bwlimit			#bandwidth limit

*log in on separate node (as snakeX)*
    .. code-block:: bash
        
        ssh -X yourAcc@snakeX

*read pdf on cluster*
    .. code-block:: bash
        
        zathura pdfname.pdf

*for internet browser (chromium) on cluster*
    .. code-block:: bash
        
        chromium -disable-gpu

*merge pdfs (not on cluster)*
    .. code-block:: bash
        
        pdftk file1.pdf file2.pdf file3.pdf cat output merged.pdf 	# it keeps bookmarks


byobu
-----
multiple persistent terminals on cluster
    .. code-block:: bash
        
        byobu       # start it on CLUSTER
        F6          # detach/ logout from byobu-session
        byobu -r    # restore last session
        F2          # start new terminal within session
        F3 & F4     # switch between open terminals
        F9          # menu
        Ctrl+A → ESC# Scrollen (nicht durch Kommandos ;))

Condor
------
.. code-block:: bash
    
    condor userprio - all -allusers #get user priorities
    condor_submit                   #generate file for submitting
    condor_q -analyze XX            #diagnosis of JobID XX
    condor_history                  #history file
    condor_status                   #load of condor
        -avail                      #only avaiable machines
        -run                        #only machines of currently runnign jobs
        -long                       #detailed informations
        -submitters                 #who uses machines
    condor_rm XX                    #remove JobID XX

==========
Bash stuff
==========

*File browser commander, like old DOS (need to install)*
    .. code-block:: bash
        
        mc 

*Find file*
    .. code-block:: bash
        
        find -iname 'filename'
        find and delete folder with its children
        find . -name foldername -type d -print0|xargs -0 rm -r --

*watch a process etc*
    .. code-block:: bash
        
        watch cmd # i.e. watch condor_q

*Install and update software*
    .. code-block:: bash
        
        aptitude
            search XsoftwarenameX
            install XsoftwarenameX
            upgrade 			#update sourcelist
            update				#do updates available
            autoclean			# delete old packages to get free space

*get help*
    .. code-block:: bash
        
        man XfuncX
        XfuncX -?
        XfuncX ?
        XfuncX -help
        XfuncX -h


*bash function*
    .. code-block:: bash
        
        function niigzBasename
        {
        ## get basename from a niftigz path or file
        # usage: niigzname=$(niiBasename $inpath)
        echo "$(basename $1 .nii.gz)"
        }

        basename $pathtofile #get filename
        dir $pathtofile #get dir to path

*extract path from absolute filepath*
    .. code-block:: bash

        path=/path/to/file/abc.de
        path=${path%/*} #  path=/path/to/file

*if else example*
    .. code-block:: bash
        
        XX=1
        [[ -z "${XX##1}" ]] && printf "XX is 1\n" || printf "XX is NOT 1\n"

*rename multiple files (replace hdr with hdr_ab12_mrVistaRaw)*
    .. code-block:: bash
        
        for f in *_hdr.nii.gz; do mv "$f" "${f/hdr/hdr_ab12_mrVistaRaw}" ; done

*set up a list to load via a for-loop*
    ``http://stackoverflow.com/questions/2654894/convert-numbers-to-enumeration-of-strings-in-bash``
    
    .. code-block:: bash
        
        idx=WeekdayWanted
        day=([1]=Monday Tuesday Wednesday)
        echo ${day[idx]}

*basic commands*
    .. code-block:: bash

        chmod +x file.sh # make file as bashfile executeable    
        du -hcs foldername		#get size of folder
        clear 				#clear screen
        mkdir 				#make dir
        rm -rf				#remove dir
        ps 				#list running processes
        xload 				#GUI for system load
        eog				#viewer for pictures

        kill PROCESS 		#kill PROCESS
        bg PROCESS 			#put PROCESS to background
        fg PROCESS 			#put PROCESS to foreground

        rm 				#remove
            file1 			#remove file1
            i file1 			#remove file12 with prompt for deleting
            -r dir1 		#remove dir1
            -rf 			#delete all files and folders contains in lampp directory	

        ls 				#list of files
            -a 			#show hidden files
            -l 			#more detailed list

        less 				#scrollable list

        cp 				#copy files and dirs
            file1 dir1 		#copy file1 to dir1
            -r -v dir1 dir2 		#copy dir1 to dir2
            -s 			#copy symlinks by making links to it, thus a link to a link
            -r			# copy folders with subfolders (recursively)
            -L			# dereference links, thus copy the original file the link points at
        mv 				#move
        du 				#disk usage
        cd 				#change dir
        pwd 				#get present path
        sort 				#sort filelist 201, 1001 here 1001, 201
        sort -V				# sort 201, 1001 here 201, 1001
        head -n5 			#take first 5
        sed -e 's/^/ -overlay /g' 	#add '-overlay' in every new line

        COMMAND > filename1 	#write COMMAND to file with filename1
        i.e. ls > file1.txt 		#writes list from ls to file1.txt
        ls >> file1.txt 			#append list to file1.txt
        COMMAND < file1.txt 	#take input from file1.txt and use COMMAND on it
        CMD1 | CMD2 		#pipe out from CMD1 to CMD2 (find *.* | sort)
        
        echo $VAR 			#show system variable VAR (i.e. for freesurfer)
        which CMD 			#shows folder of executable of CMD

        gedit filename 		#edit file with gedit (works for most software)

    Ctrl+D # close Programm, log off
    Ctrl+Z # suspend (not kill) process

*Environmentvariables*
    .. code-block:: bash

        gedit .bashrc			#manipulate environment variables
        .~/.bashrc 			#reload bachrc (after editing)
        passwd 			#change password for active user

*unpack tgz*
    .. code-block:: bash    

        tar zxvf fileNameHere.tgz

*pack/zip with password*
    .. code-block:: bash

        zip --password GiveApwd outname.zip infile 	# to zip a file
        zip -r --password GiveApwd outname.zip infolder 	# tp zip a folder

*Regular Expressions (for searching, file selection etc.)*

    .. code-block:: bash
    
        Data??? 			# 'Data' and exactly 3 more sign following
        [abc]* 				#every file beginning with 'a', 'b', or 'c', followed by other char
        [[:upper:]]* 			#beginning with uppercase char
        BLUB[[:digit:]][[:digit:]] 	#start with BLUB and followed bei 2 digits
        [![:lower:]] 			#any filename NOT ending with lowercase letter
        s/^/ 				#new ( ^/) line ('s/)

    .. code-block:: bash

        man dash # overview for examples as:
        print '${var#.nii*}'    # print var without the ending „.nii.“ and everything behind
        #    # : from the back
        #    % : from the front
        #    single: # : smallest matching; double: ## : biggest matching


*search for something in file or output*
    ``http://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/``
    
    .. code-block:: bash
    
        grep 'word' filename
        grep 'word' file1 file2 file3
        grep 'string1 string2'  filename
        cat otherfile | grep 'something'
        command | grep 'something'
        command option1 | grep 'data'
        grep --color 'data' fileName
        to search „-Z“ in a –help output:
        command –help | grep '\-Z' # '\' for search ing for the '-'

*delete files found*
    .. code-block:: bash
        
        find . -name '*filetofind*' -delete

*replace part of a name in a file*
    .. code-block:: bash
        
        for i in * ; do mv -v "$i" "sub-01${i#cw76}"; done

*see all files containing „house“ but not containing „_“*
    ``http://www.unix.com/shell-programming-and-scripting/100038-ls-exclude-pattern.html``
    
    .. code-block:: bash
    
        ls house*| grep -v "_"

*replace a word in all files*
    .. code-block:: bash
        
        for ii in $(ls -1d hypal*GM.cfg); do sed -i "s/${old}/${new}/g" $ii; done

*check dim4 for all files in subfolders*
    .. code-block:: bash
    
        for files in $(ls -1d sub0*/BOLD/task001_run00*/*bold7Tp1_to_subjbold3Tp2.nii.gz); do echo $files; fslinfo $files |grep dim4; done


======
Python
======

Common
------

*set a new pythonpath*
    .. code-block:: bash
        
        PYTHONPATH=/your/path/to/the/toolbox/here

*split code across lines*
    .. code-block:: python

        a = \	# use \
                b + c

*start more interactive python*
    .. code-block:: bash
        
        ipython -wx

*adress multiple lines in array*
    .. code-block:: python

        b = a[[2,5,20],:]

*run file and exit ipython afterwards without asking for exit*
    .. code-block:: bash
        
        ipython filename.py -noconfirm_exit 

    .. code-block:: python

        %reset 			#reset workspace
        %timeit XX 			#duration of function XX

*import specific function*
    .. code-block:: python

        from module import function

*reimport the function after it changed (without restarting python 3.X)*
    .. code-block:: python

        import imp
        imp.reload(modulename)

*Digits*
    .. code-block:: python

        1/6 = 0
        1./6 = 0.16666...

*function example*
    .. code-block:: python

        def a_func(a_var):
            b = a_var + 1
            return b

*to calculate with different arrays keeping digits*
    .. code-block:: python

        numpy.double(array1)/array2

*more basic stuff*
    .. code-block:: python

        #comments
        
        XX.append(YY) 		#append YY on list XX
        XX.del 			#delete elements from list
        'val' in list 			#if val is in list, return 'True', otherwise 'False'
        XX.index('val') 		#returns index 'val' occurs first
        X = [0]*len(YY) 		#creates array with zeros with length of YY
        XXX.split('a;b',';') 		#split string XXX at '';“
        xrange(X) 			#like range(X) but faster

        num2str 			#convert num to string
        int(XX) 			#XX to integer

        argwhere(x > 1) 		#find where arguments in array are bigger then 1

        try what = 1/in0
        except ERRORNAME #handle occuring errors

        print("Total score for", name, "is", score) # get "Total score for (name) is (score)

*copy list XX with all containing lists*
    .. code-block:: python

        import copy
        deepcopy(listXX) 

*debugger*
    .. code-block:: python

        debug
        import pdb
                l – list, show code
                n – next line
                s – step to next line
                r – return
                b – break
                c – continue, go to end or next breakpoint
                a – arguments

*for loop example(all values in bb, smaller then 0.5*max set to -1)*
    .. code-block:: python

        for ii in range(0,testend):
            bb[ii][bb[ii] = (max(bb[ii]*0.5)] = -1

*nameless, temporary function lambda*
    .. code-block:: python

        filter((labda x: x>3), range (-5,5))

*run py-files with input parameter from cmdline*
    .. code-block:: python

        #python tmp.py 1,2,3 coh -noconfirm_exit
        import sys
        print "filename of self", sys.argv[0]
        print "Testing output", sys.argv[1]
        import numpy as np
        a = sys.argv[1]
        in1 = int(a.split(',')[0])
        print type(in1)

*check if file is in folder*
    .. code-block:: python    
        
        checklist = [i.rfind(maskname) for i in os.listdir(mask_path)]
        if 0 in checklist:
            print 'it is in'

*get uniques in list*
    .. code-block:: python

        newList = list(set(oldList)) # http://mattdickenson.com/2011/12/31/find-unique-values-in-list-python/

*find string in list*
    .. code-block:: python

        tofind = cond+'.nii.gz'	# define part of filename to find to get particular file to 'cond'
        where = [i.endswith(tofind) for i in filelist] 			# where in list find pattern
        loadRetMap = [s for r,s in zip(w,filelist) if r is True] 	# read file for which tofind = True

*scipy*
    condensed distance matrix Y is NOT condensed distance matrix y:
    pay attention for transformation to fit output to scipy.cluster.hierarchy.linkage!!!
    needs a squared distance matrix, 
    NO distance vector like condesed distance matrix Y!1elf!! .. confusing

*mvpa2*
    .. code-block:: python

        Dataset.shape = (X, Y) → X = Volumes, Y = Voxel
        mvpa2.suite.map2nifti(RefNifti, Dataset)

*Nipype debug*
    .. code-block:: python

        from nipype.utils.filemanip import loadflat
        crashinfo = loadflat('crashdump....npz')
        %pdb
        crashinfo['node'].run()  # re-creates the crash
        pdb> up  #typically, but not necessarily the crash is one stack frame up
        pdb> inspect variables
        pdb>quit
        Display crashfile
        nipype_display_crash crashfilename
        # i.e. nipype_display_crash crash-20130702-112548-yourAcc-realigner.a0.npz

Nibabel
-------

*load, manipulate and save nifti back*
    .. code-block:: python    
        
        def AFNInifti_dimreducer(infile)
            import nibabel as nb
            # exclude AFNI sub-brick dimension to make it 
            # readable for mvpa2.suite.fmri_dataset
            fileorg = nb.load(infile)
            dataorg = fileorg.get_data()
            datared = dataorg[:,:,:,:,0] 
            outname = ''.join([infile.split('.')[-3],'_reddim.nii.gz'])
            # use header or loaded infile
            datared = datared.squeeze()
            outnifti = nb.Nifti1Image(datared, fileorg.get_affine())
            nb.save(outnifti, outname)
            return outnifti

*change TR in nibabel header*
    .. code-block:: python

        V1affine= nb.load(maskpath)
        # set TR to 2s
        TR=2
        imghdr = V1affine.get_header()
        pixdims = imghdr['pixdim']
        pixdims[4] = TR
        imghdr['pixdim'] = pixdims
        .. use thsi header for further saving

*use class*
    .. code-block:: python

            class Settings(object):
            "save all the settings for the experiment. They are taken to generate all the rest"
            def __init__(self):
                Settings.__init__(self)

            class screen():
                refreshRate = 60 # [MHz]
            
            class timing():
                time0 = 2

            def _s(inTime, refreshRate=screen.refreshRate):
                flipTime = 1./refreshRate
                return round(inTime/flipTime)*flipTime
            
            # convert initial time to multiple of refreshRate    
            timing.time0conv = _s(timing.time0)
            
            print timing.time0conv


*Pandas: select columns in a dataframe*
    .. code-block:: python

        method, ecc_pol, = "Conn", "ecc"
        selector = ('method == "%s" & ecc_pol == "%s" "' % (method, ecc_pol,))
        Dfstuff =  DF.query(selector)

*Pythonpath*
    install toolbox by adding to PYHONPATH
    
    #. git clone toolboxgitRepoURL zB: git clone ``https://github.com/beOn/cili.git``
    #. in .bashrc add path (zB /home/you/Python/cili)to cloned repo

*use own toolbox instead of local; overwrite PYTHONPATH*
    ``http://stackoverflow.com/questions/3402168/permanently-add-a-directory-to-pythonpath#3402193``
    Instead of manipulating PYTHONPATH you can also create a path configuration file. First find out in which directory Python searches for this information:
    python -m site --user-site
    For some reason this doesn't seem to work in Python 2.7. There you can use:
    python -c 'import site; site._script()' --user-site
    Then create a .pth file in that directory containing the path you want to add (create the directory if it doesn't exist).
    For example:
    
    .. code-block:: python
    
        # find directory
        SITEDIR=$(python -m site --user-site)

        # create if it doesn't exist
        mkdir -p "$SITEDIR"

        # create new .pth file with our path
        echo "$HOME/foo/bar" > "$SITEDIR/somelib.pth"

==============
Problemsolving
==============

*NIS-server fail for log in via network (at log in, after boot) (OLD) → restart NIS*
    1. Ctrl+Alt+F1 – terminal
    2. inoke-rc.de nis restart
    3. Ctrl+Alt+F7 – close terminal
    4. ping elrond – check connection
    5. mount /home

*frozen Desktop but Terminal (Ctrl+Alt+F4) working → log off from desktop*
    .. code-block:: bash

        sudo invoke-rc.d gdm3 restart

*frozen software .. kill programm*
    .. code-block:: bash

        killall softwareID (i.e. killall MATLAB)

==
i3
==

*disable autostart of nemo when connecting usb*
    .. code-block:: bash

        gsettings set org.nemo.desktop show-desktop-icons false

===========
Eyetracking
===========

Find a nice overview here: ``https://github.com/davebraze/FDBeye/wiki``

=========================
Graphic and Paining stuff
=========================

*Inkscape*
    
    - Ctrl+Shift+F    # figure options
    - Ctrl+Alt+V      # paste object in same place
    - Ctrl+Chift+V    # paste style to clipboard

ImageMagick
-----------
*convert many images, i.e. resize*
    
    ``http://www.howtogeek.com/109369/how-to-quickly-resize-convert-modify-images-from-the-linux-terminal/``
    
    .. code-block:: bash
    
        for ii in $(ls -1d *.bmp); do convert $ii -resize 34x34 $(basename $ii .bmp)_newname.bmp; done

GIMP
----
*Generate Colorcode from Colorbar (freeview)*
     Filter → Distort → Polar Coordinates: Here set rotation and everything you need 

*make transparent background*
    ``http://docs.gimp.org/en/gimp-tutorial-quickie-separate.html``
    Tools → Selection Tools → Fuzzy Select → click on background
    Layers → Transparency → Add Alpha channel → „Del“ (delete key)
    .. now should be gray checkerboard background aka transparent/ no background

==============
MRI-processing
==============

*unpack dicoms*
    .. code-block:: bash

        dinifti -d -g sourcefolderpath outfolderpath
        
        # examples
        dinifti -d -g XXX . #(dot as „put here“) or 
        $(find . -name 'MR.*) #to find all files starting with „MR“

FSL
---

*Upsampling by wolf*
    Ich hatte dich ja vorhin bezueglich des Hochsamplings an die FSL-mailingliste verwiesen (``https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=fsl;4814e67b.1110``) 
    Prinzipiell kannst du daten mit flirt resamplen, indem du keine Transformation vornimmst durch die Verwendung einer identity transformation matrix ($FSLDIR/etc/flirtsch/ident.mat). DU musst dann eine Header Datei erzeugen, die sowohl die gewuenschten Voxeldimensionen als auch das pasende FoV hat (ich habe mir dafuer das banale angehaengte Skript geschrieben). 
    Die Schritte kannst du auch angezeigt bekommen, indem du ueber die Kommandozeile ApplyXFM oeffnest, dort FoV und Voxeldimensionen spezifizierst und startest. Dann werden dir die beiden notwendigen Befehle mit de entsprechenden Argumenten in der Kommandozeile angezeigt (fslcreatehd und flirt). 
    Fuer downsampling kann man auc auf fslmaths zurueckgreifen (-subsamp2 Option). 

    Viel Glueck, 
    wolf

*adopt resolution of image2 to image2*
    .. code-block:: bash
        
        flirt -in Image1 -ref Image2 -out OutImage

*check image header of nifti*
    .. code-block:: bash
        
        fslhd filename

*extract single volume or ROI from files*
    .. code-block:: bash

        fslroi 'Coregistered_AsegToFunc/vol0000_warp_merged_detrended_regfilt_filt_warped.nii.gz' coregaseg_vol50.nii.gz 50 1

*expand ROI (grow ROI in size, at borders)*
    .. code-block:: bash

        fslmaths INFILE -kernel sphere 3 -dilD OUTFILE.nii.gz'

*get binary mask*
    .. code-block:: bash

        fslmaths In_nonbinary.nii.gz -thr 50 -bin out_binary.nii.gz

*extract brain (skullstrip & brainmask)*
    .. code-block:: bash

        bet in_fullAnat.nii.gz out_brain.nii.gz -f 0.3 -g 0.1 -m

*MELODIC*
    each regressor should be as a component in the ICA, if separable only this one is significant in last table

Freesurfer
----------

*process anatomy bigger than 256 in one dimension → crop image*
    .. code-block:: bash

        fslroi 'infile.nii.gz' 'outfile.nii.gz' 0 -1 10 256

*tksurfer & freeview*
    * project on pial surface 
        .. code-block:: bash

            tksurfer ab12_nipype lh pial 
        
        -> then load overlay

    * load inflate and project on this inflate surface an overlay
        .. code-block:: bash

            tksurfer ab12 lh inflated $(find /where/you/want/to/search/ -name '*.mgz' -printf ' -overlay %p' | sort)
            freeview --surface /path/to/Freesurfer/subID/surf/lh.inflated:overlay=/path/toOverlay/lh.overayName.mgz

*transform fsl-coregistration matrix (.mat) to freesurfer-coreg-matrix (.dat)*
    .. code-block:: bash

        tkregister2 --mov 'path/to/referenceNifti.nii.gz' --fsl 'path/to/fslCoregMatrixName.mat'  --s subID --reg newFreesurferMatrix.dat --noedit

*project on surface*
    .. code-block:: bash

        mri_vol2surf --hemi lh --mov 'referenceNifti.nii.gz' --reg '/path/to/coregMat.dat' --projfrac-avg 0.000 0.75 0.05 --out '/path/to/new/surfaceFile-lh.w' --out_type paint

*convert mgz to nifti*
    .. code-block:: bash

        mri_convert aseg.mgz aseg.nii.gz

*merge 2 mgz-files*
    .. code-block:: bash

        mri_concat --i '/file/A/to/merge/lh.ribbon.mgz' '/file/B/to/merge/rh.ribbon.mgz' --o merge.mgz –combine

*merge several img&hdr to a 4D nifti*
    .. code-block:: bash
        
        mri_concat $(find . -name 'raf*.img') --o test_hdr.nii.gz 
        concat for all raf*.img scans in all subfolder scan*
        for ii in scan*; do mri_concat $(find $ii/ -name 'raf*.img' | sort) --o ${ii}_hdr.nii.gz; done

*do recon-all for several subjects*
    .. code-block:: bash

        for ii in $(ls -1d /path/to/your/subjects/sub*); do
            sID=$(basename $ii) #get filemame
            recon-all -i $ii/anatomy/highres001.nii.gz -cw256 -all -T2 
            $ii/anatomy/other/t2w001.nii.gz -T2pial -subjid $sID -sd 
            /path/to/your/FreesurferSubjects/Folder
        done

*do flirt for several subjects*
    .. code-block:: bash

        first try „Flirt“ to get the GUI. Use the output to terminal to generate a script

*some examples*
    .. code-block:: bash

        ## bring func to template space
        # get transformation matrix to coreg funcMoCo2templ
        flirt -in subID/BOLD/task001_run001/bold_ref.nii -ref templates/task001/brain.nii.gz -out subID/funcref2templ.nii.gz -interp nearestneighbour -omat subID/funcref2templ.mat
        # use coregmat on func (MoCo)
        flirt -in subID/MoCo.nii.gz -ref templates/task001/brain.nii.gz -out subID/MoCo2templ.nii.gz -interp nearestneighbour -init subID/funcref2templ.mat -applyxfm

        flirt -in subID/MoCo_3DBandpass.nii.gz -ref templates/task001/brain.nii.gz -out sub_ft69/MoCo2templ.nii.gz -interp nearestneighbour -init subID/funcref2templ.mat -applyxfm
               
        # do above for all subjects
        for ii in $(ls -1d /home/yourAcc/MRI/RestingState_RetMapping/Hyperalign_Maps/hyperalignPreProc_out/sub_*)
        do
        sID=$(basename $ii) #get filemame
        flirt -in $ii/BOLD/task001_run001/bold_ref.nii -ref templates/task001/brain.nii.gz -out $ii/funcref2templ.nii.gz -interp nearestneighbour -omat $ii/funcref2templ.mat
        flirt -in $ii/MoCo.nii.gz -ref templates/task001/brain.nii.gz -out $ii/MoCo2templ.nii.gz -interp nearestneighbour -init $ii/funcref2templ.mat -applyxfm
        flirt -in $ii/MoCo_3DBandpass.nii.gz -ref templates/task001/brain.nii.gz -out $ii/MoCo2templ.nii.gz -interp nearestneighbour -init $ii/funcref2templ.mat -applyxfm
        done

===
Git
===

*more basics*
    .. code-block:: bash

        git
            init 				#initialize git repository in present folder
            add file1			#add file1 to repository
            commit file1			#update rep. Of file1 with present version
            diff –-staged 			#show changes
            branch 'branchname'		#split up under new name
            log 				#overview
            config –-global core.editor 'nano' 	#set editor to use for comments etc
            clone 'source' 'aim'
            revert COMMITID_TOGOTO #restore last backup (^^ #go 2 steps back)
        mv.git / file1 file2 newfolder/ 	#move gitrepository with files1&2 to newfolder

*commit line by line*
    go line by line through changes and commit them line by line. 
    Possible to just commit parts of your last changes to separate different fixes into different commits
    
    .. code-block:: bash

        git add -p 

*link to issues*
    .. code-block:: bash

        add #111 to text in issue, commit message or where ever and it will be linked to the issue 111

*restore older commited verison*
    .. code-block:: bash
        
        git checkout -- filename

*Load and update repository*
    .. code-block:: bash
        
        git clone Forest@CLUSTER git # get it
        update
        go in repository folder 
        git pull
        make #to generate newest pdf from tex

*make a new branch an switch to it*
    .. code-block:: bash

        git branch XXX    # make new branch named XXX
        git checkout XXX   # switch to branch XXX. To go back to „master“: git checkout master
        git branch –list       # get a list of all branches available
        git merge XXX      # merge branch XXX with the branch YOU ARE IN AT THE MOMENT (evtl. switch to master branch beforehand, if you want to merge with master

*um änderungen offiziell einzufügen*
    .. code-block:: bash

        git commit filename
        git push (origin) #here wothout name

*merge changes of different authors*
    .. code-block:: bash

        git add -p filename
        git commit
        git push

*if out of date (merging)*
    .. code-block:: bash

        git pull
        „commit“ (if easy)
        git push

*if conflict*
    .. code-block:: bash

        edit file
        resolve by hand → in file search for „>>>“ 
        → shows my old and the others „new“ entries → delete my or the others
        git add filename
        git commit
        git push 

*to find word in changes*
    .. code-block:: bash

        serach hidden .git folder for this word → get branch to search in for it
        grep -Ri hyp .git/

*make git bare repository to sync all other repos with*
    .. code-block:: bash

        mkdir newreopdirname
        cd newreopdirname
        git init --bare --shared=group ./
        chgrp -R yourgroup newreopdirname # the group is your workgroup or which group should be able to access the repo
        OR 
        chmod 775  newreopdirname

==========
MATLAB-fun
==========

*run PTB without root (needs root to setup)*
    #. su 	#login as su
    #. sudo adduser yourAccount sudo # add user to sudeors list
    #. REBOOT
    #. start matlab: → ScreenTest; PsychLinuxConfiguration → answer all with y, enter yourACC-Password when asked
    #. REBOOT
    
    matlab and PTB shpuld now be able to send triggers and handle deep system settings for your presentation 

*debug*
    .. code-block:: matlab

        debug - dbstop
        dbup 			% go one function up
        dbdown 		% go function down

*code optimization*
    .. code-block:: matlab

        profile on
        ..run your code ..
        profile off
        profile viewe % same as „Run and Time“ frim editor

========
HARDWARE
========

*120 Hz DVI*
    only works with Dual-link DVI cable. These are thicker then „normal“ and it should be written on the cable: DUAL-LINK DVI
