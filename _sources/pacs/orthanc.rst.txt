=======
Orthanc
=======

A open source lightweight DICOM server. It has a a plugin mechanism to add modules. see its `official page <https://www.orthanc-server.com/static.php?page=about>`_ for more information.

Installation
============

#. Update update package repositories.

    .. code-block:: bash

        sudo apt-get update -y

#. Install the packages and dependencies.

    .. code-block:: bash

        sudo apt-get install -y orthanc

#. Now, orthanc is installed. Go to http://localhost:8042/app/explorer.html, where port number is defined by your configuration (the default http port of orthanc is ``8042``).
#. Install plugins

    #. DICOMweb: provide support of the DICOM web standard.

        .. code-block:: bash

            sudo apt-get install -y orthanc-dicomweb

    #. Orthanc Web Viewer: provide a web viewer of medical images.

        .. code-block:: bash

            sudo apt install orthanc-webviewer

    #. Osimis Web Viewer: an advanced image viewer

        * Option 1: Install & set up locally. See `here <https://book.orthanc-server.com/plugins/osimis-webviewer.html#how-to-get-it>`_ for more information.

            * Download the LSB (Linux Standard Base) binaries. `Download <http://orthanc.osimis.io/lsb/plugin-osimis-webviewer/releases/1.3.1/libOsimisWebViewer.so>`_.
            * Move/copy the .so file to /usr/share/orthanc/plugins.
            * Restart Orthanc

                .. code-block:: bash

                    sudo /etc/init.d/orthanc restart --verbose

            * Go to http://localhost:8042/app/explorer.html
            * Default username is orthanc and password is orthanc

        * Option 2: Setup using docker image (need to have Docker installed). See `Osimis Installation guide <https://osimis.atlassian.net/wiki/spaces/OKB/pages/79167642/Osimis+Web+Viewer+-+Installation+guide>`_ or `Osimis Quickstart <https://osimis.atlassian.net/wiki/spaces/OKB/pages/26738689/How+to+use+osimis+orthanc+Docker+images#Howtouseosimis/orthancDockerimages?-Quickstart>`_ for more information.

            * run

                .. code-block:: bash

                    sudo docker run -p 8042:8042 -p 4242:4242 -e WVB_ENABLED=true -v /home/username/orthanc-storage:/var/lib/orthanc/db osimis/orthanc

            * Go to http://localhost:8042
            * Default username is ``orthanc`` and password is ``orthanc``.

        .. note::

            The basic Orthanc Web Viewer can only access images from series level, while Osimis can access images from study and series levels. They can coexist.

#. (optional) Change configurations

    #. Create a configuration file if it does not exist.

        .. code-block:: bash

            sudo Orthanc --config=/etc/orthanc/configuration.json

        * Where /etc/orthanc/ is the folder stored the Orthanc main configuration and plugin configurations.

    #. Modify the related configuration file and fields.
    #. Restart Orthanc

        .. code-block:: bash

            sudo /etc/init.d/orthanc restart --verbose

Uploading files
===============

Uploading through the DICOM protocol(recommended)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Upload a directory

    .. code-block:: bash

        sudo storescu -v IP 4242 -aec ORTHANC --scan-directories /pathToDir

    .. note::

        Port number 4242 is Orthanc’s default dicom port.

* Upload a single dicom file

    .. code-block:: bash

        sudo storescu -aec ORTHANC 4242 /pathToDicomFile

Uploading through web browser (drag and drop)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Files can also be upload via the web GUI from http://localhost:8042/app/explorer.html#upload

Troubleshooting
===============

Where to find the log file?
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Everytime the Orthanc service starts, it will generate a new log file in /var/log/orthanc/ for Linux or in C:\Program Files\Orthanc Server\Logs for Windows.

Where to find the configuration files?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In /etc/orthanc/. You can run the command below to to create a orthanc main configuration file and attach to the orthanc server. Each plugin also has its own configuration file inside the folder.

.. code-block:: bash

    sudo Orthanc --config=/etc/orthanc/configuration.json

Failed to restart Orthanc after adding the osimis viewer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You might need to add the read permissions of the the .so file by:

.. code-block:: bash

    chmod o+r /usr/share/orthanc/plugins/libOsimisWebViewer.so

If you start orthanc with --verbose parameter, you will be able to view the log in /var/log/orthanc/Orthanc.log

How does Orthanc store its database?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See https://book.orthanc-server.com/faq/orthanc-storage.html

