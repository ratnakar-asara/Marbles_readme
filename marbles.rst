Marbles Demo
============

The marbles chaincode application demonstrates the ability to create
assets (marbles) with unique attributes - size, color, owner, etc. - and
then trade these assets with fellow participants in a blockchain network.

Getting Setup
-------------

Make sure you have the following prerequisites installed on your machine:

-  `Go <https://golang.org/>`__ - most recent version
-  `Docker <https://www.docker.com/products/overview>`__ - v1.13 or
   higher
-  `Docker Compose <https://docs.docker.com/compose/overview/>`__ - v1.8
   or higher
-  `Node.js & npm <https://nodejs.org/en/download/>`__ - node v6.2.0 - v6.10.0 (v7+ not supported); npm
   comes with your node installation.
-  `xcode <https://developer.apple.com/xcode/>`__ - only required for OS
   X users
-  `nvm <https://github.com/creationix/nvm/blob/master/README.markdown>`__ - if you
   want to use the ``nvm install`` command to retrieve a node version

If you already have node on your machine, use the node website to upgrade or issue the
following command in your terminal:

.. code:: bash

       # you can choose any available version that is 6.2.0 < 7
       nvm install v6.10.0

Then execute the following to see your version:

.. code:: bash

       # should be 6.2.0 < 7
       node -v

Clone the repos
^^^^^^^^^^^^^^^

-  Determine a location on your local machine where you want to place
   the Marbles and SDK libraries.  For example:

.. code:: bash

       mkdir -p $HOME/<my_dev_workspace>
       cd $HOME/<my_dev_workspace>

- Next, from your workspace, clone the two repos:

.. code:: bash

    git clone https://github.com/hyperledger/fabric-sdk-node.git
    git clone http://gopkg.in/ibm-blockchain/marbles.v3

Download the docker images
^^^^^^^^^^^^^^^^^^^^^^^^^^

-  Download the `cURL <https://curl.haxx.se/download.html>`__ tool if
   not already installed.

Next, from your chosen workspace, execute the following command:

.. code:: bash

       curl -OO https://raw.githubusercontent.com/bmos299/fabric/rtd1/examples/e2e_cli/{download-dockerimages.sh,docker-compose-marblesv3.yaml}

This command pulls a shell script that will download and extract the necessary docker
images.  It also pulls the docker-compose file that we will use to spawn our network.

Your directory should now contain the following:

.. code:: bash

      jdoe-mbp:<my_dev_workspace> johndoe$ pwd
      /Users/johndoe/<my_dev_workspace>
      jdoe-mbp:<my_dev_workspace> johndoe$ ls
      docker-compose-marblesv3.yaml	download-dockerimages.sh fabric-sdk-node marbles.v3

.. note::  If you already ran through the getting started tutorial, then you
           needn't worry about pulling the alpha images
           (i.e. using the below download-dockerimages.sh script).  You can skip
           directly to the "Update your directories" section.

From your workspace, make the shell script an executable:

.. code:: bash

      chmod +x download-dockerimages.sh

Now run the script.  Make sure you have docker running before executing this script.
This process will take a few minutes so be patient:

.. code:: bash

      ./download-dockerimages.sh

Once the script has completed, you should see the following in your terminal:

.. code:: bash

      ===> List out hyperledger docker images
      hyperledger/fabric-ca          latest               35311d8617b4        3 weeks ago         240 MB
      hyperledger/fabric-ca          x86_64-1.0.0-alpha   35311d8617b4        3 weeks ago         240 MB
      hyperledger/fabric-couchdb     latest               f3ce31e25872        3 weeks ago         1.51 GB
      hyperledger/fabric-couchdb     x86_64-1.0.0-alpha   f3ce31e25872        3 weeks ago         1.51 GB
      hyperledger/fabric-kafka       latest               589dad0b93fc        3 weeks ago         1.3 GB
      hyperledger/fabric-kafka       x86_64-1.0.0-alpha   589dad0b93fc        3 weeks ago         1.3 GB
      hyperledger/fabric-zookeeper   latest               9a51f5be29c1        3 weeks ago         1.31 GB
      hyperledger/fabric-zookeeper   x86_64-1.0.0-alpha   9a51f5be29c1        3 weeks ago         1.31 GB
      hyperledger/fabric-orderer     latest               5685fd77ab7c        3 weeks ago         182 MB
      hyperledger/fabric-orderer     x86_64-1.0.0-alpha   5685fd77ab7c        3 weeks ago         182 MB
      hyperledger/fabric-peer        latest               784c5d41ac1d        3 weeks ago         184 MB
      hyperledger/fabric-peer        x86_64-1.0.0-alpha   784c5d41ac1d        3 weeks ago         184 MB
      hyperledger/fabric-javaenv     latest               a08f85d8f0a9        3 weeks ago         1.42 GB
      hyperledger/fabric-javaenv     x86_64-1.0.0-alpha   a08f85d8f0a9        3 weeks ago         1.42 GB
      hyperledger/fabric-ccenv       latest               91792014b61f        3 weeks ago         1.29 GB
      hyperledger/fabric-ccenv       x86_64-1.0.0-alpha   91792014b61f        3 weeks ago         1.29 GB

Update your directories
^^^^^^^^^^^^^^^^^^^^^^^

.. note:: You must operate from your workspace directory (where you cloned the
          repos and curled the scripts) in order for the following command
          structures to work.

First, checkout the alpha branch of the ``fabric-sdk-node`` repository:

.. code:: bash

      cd fabric-sdk-node
      git checkout v1.0.0-alpha

Ensure that you are on the correct branch:

.. code:: bash

      git branch

You should see the following:

.. code:: bash

      jdoe-mbp:fabric-sdk-node johndoe$ git branch
      * (HEAD detached at v1.0.0-alpha)
      master

Now hop back to your workspace directory:

.. code:: bash

      cd ..

From your workspace, move the ``docker-compose-marblesv3.yaml`` to the ``test/fixtures``
folder in the ``fabric-sdk-node`` directory:

.. code:: bash

      mv docker-compose-marblesv3.yaml fabric-sdk-node/test/fixtures

Still from your workspace, empty the example chaincode source from the ``fabric-sdk-node`` directory:

.. code:: bash

      rm -rf fabric-sdk-node/test/fixtures/src/github.com/example_cc/*

Now copy the marbles source to that same folder:

.. code:: bash

      cp marbles.v3/chaincode/src/marbles/* fabric-sdk-node/test/fixtures/src/github.com/example_cc/

Edit the configuration
^^^^^^^^^^^^^^^^^^^^^^

.. note:: Continue operating from your workspace directory.

Update the ``config.json`` and ``instantiate-chaincode.js`` files in the ``fabric-sdk-node`` directory:

.. code:: bash

      cd fabric-sdk-node/test/integration/e2e

Use an editor to open the ``config.json`` and replace all instances of ``grpcs``
with ``grpc``.

Use an editor to open ``instantiate-chaincode.js`` and replace line 147 with:

.. code:: bash

      args: ['100'],

Navigate to the marbles repo and edit the credentials file:

.. code:: bash

      cd $HOME/<my_dev_workspace>/marbles.v3/config/

Use an editor to open ``blockchain_creds1.json``.  Replace the "network_id" field with
a name of your choice.  Change "chaincode_id" field to ``end2end``.  Change the
"chaincode_version" field to ``v1``.

Make sure to save all of your changes before continuing.

Start your network
------------------

Now for the fun stuff.  Navigate to the ``test/fixtures`` folder in the ``fabric-sdk-node`` directory and run the docker-compose file:

.. code:: bash

      cd ../../fabric-sdk-node/test/fixtures
      docker-compose -f docker-compose-marblesv3.yaml up -d

It will most likely pull a couchDB image before starting your containers.  Once
complete, issue a ``docker ps`` command to view your currently running containers.
You should see the following:

.. code:: bash

   CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS                       PORTS                                            NAMES
   e61cf829f171        hyperledger/fabric-peer      "peer node start -..."   3 minutes ago       Up 2 minutes           0.0.0.0:7056->7051/tcp, 0.0.0.0:7058->7053/tcp   peer1
   0cc1f5ac24da        hyperledger/fabric-peer      "peer node start -..."   3 minutes ago       Up 2 minutes        0.0.0.0:8056->7051/tcp, 0.0.0.0:8058->7053/tcp   peer3
   7ab3106e5076        hyperledger/fabric-peer      "peer node start -..."   3 minutes ago       Up 3 minutes        0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp   peer0
   2bc5c6606e6c        hyperledger/fabric-peer      "peer node start -..."   3 minutes ago       Up 3 minutes        0.0.0.0:8051->7051/tcp, 0.0.0.0:8053->7053/tcp   peer2
   513be1b46467        hyperledger/fabric-ca        "sh -c 'fabric-ca-..."   3 minutes ago       Up 3 minutes        0.0.0.0:8054->7054/tcp                           ca_peerOrg2
   741c363ba34a        hyperledger/fabric-orderer   "orderer"                3 minutes ago       Up 3 minutes        0.0.0.0:7050->7050/tcp                           orderer0
   abaae883eb13        couchdb                      "tini -- /docker-e..."   3 minutes ago       Up 3 minutes        0.0.0.0:5984->5984/tcp                           couchdb
   2c2d51fe88c0        hyperledger/fabric-ca        "sh -c 'fabric-ca-..."   3 minutes ago       Up 3 minutes        0.0.0.0:7054->7054/tcp                           ca_peerOrg1

Use the SDK
-------------

With our network up and running, we'll now use the Node SDK to issue some commands.
Hop back to the root of the ``fabric-sdk-node`` directory:

.. code:: bash

      cd ../..
      npm install

The ``npm install`` command will put the node_modules into your SDK repo.  This
will take a minute or two.  Next, install gulp:

.. code:: bash

      npm install -g gulp
      # if you get a "permission denied" error, then try with sudo
      sudo npm install -g gulp

Finally, build the fabric-ca client:

.. code:: bash

      gulp ca

Create, Join, Install, Instantiate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before starting let's remove the key value stores and hfc artifacts that may have
cached during previous runs:

.. code:: bash

      rm -rf /tmp/hfc-*
      rm -rf ~/.hfc-key-store

Create channel
~~~~~~~~~~~~~~

Now, leverage the SDK test program to create a channel named ``mychannel``.  From
the ``fabric-sdk-node`` directory:

.. code:: bash

      node test/integration/e2e/create-channel.js

Join channel
~~~~~~~~~~~~

Pass the genesis block - ``mychannel.block`` - to the ordering service and join
the peers to your channel:

.. code:: bash

      node test/integration/e2e/join-channel.js

Install chaincode
~~~~~~~~~~~~~~~~~

Install the marbles source code on the peers' filesystems:

.. code:: bash

      node test/integration/e2e/install-chaincode.js

Instantiate chaincode
~~~~~~~~~~~~~~~~~~~~~

Spin up the marbles containers:

.. code:: bash

      node test/integration/e2e/instantiate-chaincode.js


Run the marbles app
-------------------

Navigate to the marbles directory:

.. code:: bash

      cd ../marbles.v3
      npm install

Now launch the application:

.. code:: bash

      gulp marbles1

Use the UI
^^^^^^^^^^

Open a browser and visit ``localhost:3001``.  Scroll down to the bottom of the
page and login as "admin".  You're all set!  Now you can create and trade marbles.

See the logs
^^^^^^^^^^

Open another terminal and view your peer or orderer logs:

.. code:: bash

      docker logs -f peer0
      # control + c will exit the process
      docker logs -f orderer0

Troubleshooting
---------------

- If you see a "permission denied" response upon issuing ``./download-dockerimages.sh``, make sure
to turn the bash script into an executable.  From the same directory where this file is stored,
issue the following:

.. code:: bash

      chmod +x download-dockerimage.sh
      ./download-dockerimages.sh

- If you see a "containerID already exists" upon running ``docker-compose up``, then you need to remove
the existing container.  This command will remove all containers; NOT your images:

.. code:: bash

      docker rm -f $(docker ps -aq)

Now spin your network up again...

- When running ``create-channel.js``, if you see an error stating "private key not found", then try
clearing your cached key value stores:

.. code:: bash

      rm -rf /tmp/hfc-*
      rm -rf ~/.hfc-key-store

Now try again...

- When running ``instantiate-chaincode.js``, if you see an error stating "chaincodeID does not exist", then
make sure you accurately updated the ``blockchain_creds1.json`` file.  The "chaincode_id" field should
read ``end2end`` and the "chaincode_version" field should read ``v1``.

- When running ``instantiate-chaincode.js``, if you see an error stating "incorrect number of arguments, expecting
1", then make sure you accurately updated line 147 of this file.  It should read:

.. code:: bash

      args: ['100'],

- When running marbles, if you see errors similar to the following:

.. code:: bash

      error: [fcw] Failed to receive block event within the timeout period
      error: [fcw] Failed to receive block event within the timeout period
      error: [fcw] Failed to receive block event within the timeout period

      [2:03]
      debug: [fcw] listening to event url grpc://localhost:7053
      error: [Peer.js]: GRPC client got an error response from the peer “grpc://localhost:7051”. Error: Owner does not exist - o01492019568603SPxwE

Then you need to update the "block_delay" variable in the ``blockchain_creds1.json`` file.
Change this value from ``1000`` to ``10000`` and reissue the ``gulp marbles1`` command.

Still stuck?  Try posting in the **# fabric questions** channel on Rocket Chat.
