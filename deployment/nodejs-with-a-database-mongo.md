# Deploy a Node.js + Mongo DB database
This assumes you have a MongoDB server running locally. If you don't head to the download page [Download page](https://www.mongodb.com/download-center/community?jmp=docs) and click `Download`

Thereafter double click the installation file and find the resulting unpacked directory. 

Navigate to the directory and run `pwd`. Thereafter set it up in the PATH variable, like so:

/Users/chnoring/Downloads/mongodb-osx-x86_64-4.0.5

```
export PATH=<mongodb-install-directory>/bin:$PATH
```

Thereafter we need to set up the data directory where MongoDB will read and write from. We do that with:

```
mkdir -p /data/db
```

Thereafter start `mongod`, this will start the background process

Navigate t
