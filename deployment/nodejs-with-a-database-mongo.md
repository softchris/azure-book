# Deploy a Node.js + Mongo DB database
This assumes you have a MongoDB server running locally. 

## Installing MongoDB
If you already have MongoDB installed and working, skip this step otherwise keep reading.

Go to the download page [Download page](https://www.mongodb.com/download-center/community?jmp=docs) and click `Download`

Thereafter double click the installation file and find the resulting unpacked directory. 

### Set the path

Navigate to the directory and run `pwd`. Thereafter set it up in the PATH variable, like so:

/Users/chnoring/Downloads/mongodb-osx-x86_64-4.0.5

```
export PATH=<mongodb-install-directory>/bin:$PATH
```

### Create data directory and set access rights

Thereafter we need to set up the data directory where MongoDB will read and write from. We do that with:

```
mkdir -p /data/db
```

Then you need to ensure your data directory has the correct read/write access. Type the following:

```
sudo chmod -R go+w /data/db
```

### Start the engine

Thereafter start `mongod`, this will start the background process

## Get the sample app

Let's get the sample app

```
git clone https://github.com/Azure-Samples/meanjs.git
```

Thereafter

```
cd meanjs
npm install && npm start
```

Now you should get the following if you navigate to `http://localhost:3000`
![](/assets/Screen Shot 2019-01-11 at 00.53.10.png)

