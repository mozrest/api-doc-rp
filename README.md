
<p align="center">
  <img src="https://mozrest-statics.s3.eu-west-1.amazonaws.com/logos/logo.svg" alt="Mozrest" height="80">
</p>

Mozrest API Doc for Reservation Management Systems
--------------------------------------------------
<p align="center">Mozrest API Documentation is based on <a href="https://github.com/slatedocs/slate">Slate</a></p>

Getting Started with Slate
------------------------------

To get started with Slate, please check out the [Getting Started](https://github.com/slatedocs/slate/wiki#getting-started)
section in our [wiki](https://github.com/slatedocs/slate/wiki).

We support running Slate in three different ways:
* [Natively](https://github.com/slatedocs/slate/wiki/Using-Slate-Natively)
* [Using Vagrant](https://github.com/slatedocs/slate/wiki/Using-Slate-in-Vagrant)
* [Using Docker](https://github.com/slatedocs/slate/wiki/Using-Slate-in-Docker)

How to use Docker to develop
---------------------------------
### Running Slate using Docker

If you wish to run the development server for Slate to aid in working on the site, run:

```
docker run --rm --name slate -p 4567:4567 -v $(pwd)/source:/srv/slate/source slatedocs/slate serve
```

and you will be able to access your site at http://localhost:4567 until you stop the running container process.


### Building Slate

To use Docker to just build your site, run:

```
**docker run --rm --name slate -v $(pwd)/build:/srv/slate/build -v $(pwd)/source:/srv/slate/source slatedocs/slate build**
```

After this command completes, you should see the built artifacts for your site in the `$(pwd)/build` directory, which you can then statically serve for your website.

_Note_: You may omit the final `build` argument and get the same result. By default, if given no command, the Dockerfile will run `build`.

How to deploy a new version
---------------------------

Mozrest API Doc is served from AWS S3 Bucket.

To deploy a new version, you need to upload the content inside of the `build` directory to <a href="https://s3.console.aws.amazon.com/s3/buckets/doc.mozrest.com?region=eu-west-1&prefix=api/rms/&showversions=false">doc.mozrest.com/api/rms</a> bucket. 
