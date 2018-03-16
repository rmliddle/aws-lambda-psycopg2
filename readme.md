# AWS Lambda psycopg2 (python3)
AWS Linux compiled package for postgresql-10.2 and psycopg2-2.7.4

You can also make your own compiled psycopg2 package!
To start you'll need access to an AWS Linux instance, the easiest ways to get 
access to this are via AWS [EC2](https://aws.amazon.com/ec2/) 
OR Alternatively, via the offical AWS Linux [Docker](https://hub.docker.com/_/amazonlinux/)

Then, simply install your favourite python version i.e. 3.6
and install via: ```sudo yum install python36``` 

The package itself can be compiled by the following procedure:
1. Retrieve relevant source code, here it is for postgresql-10.2 and psycopg2-2.7.4
but can be replaced with other versions as needed:

    ```wget https://www.postgresql.org/ftp/source/v10.2/postgresql-10.2.tar.gz .```

    ```wget http://initd.org/psycopg/tarballs/PSYCOPG-2-7/psycopg2-2.7.4.tar.gz .```

2. Unpack both archives:
  
    ``` 
        tar -xvzf /postgresql-10.2.tar.gz
    ```

    ```
        tar -xvzf /psycopg2-2.7.4.tar.gz
    ```


3. Move into the postgres source directory:

    ```
        cd postgresql-10.2
    ```

    ```
        export POST_SRC=`pwd` 
    ```

Run the following commands:


    ``` 
    ./configure --prefix $POST_SRC --without-readline --without-zlib 
    ```


    ```
    make
    ```


    ```
    make install
    ```

4. Move to the psycopg2 directory:

    ```cd ../psycopg2-2.7.4```

    Modify ```setup.cfg``` with the following:

    ```static_libpq=1```

    ```pg_config=MY_POSTGRES_SRC/bin/pg_config```

    where the value MY_POSTGRES_SRC has been replaced by the postgres directory location. This can be retrieved by running:

    ```echo $POST_SRC```


5. (python3). AWS Linux does not ship with the required python3 development package required here, run:

    ```yum search python3 | grep devel```

    Select the desired python version development package and install, for example:

    ```sudo yum install -y python36-devel.x86_64```

6. Run ```python3 setup.py build```

7. Your desired build is now in the ```build/lib.linux-x86_64-3.6/psycopg2``` directory. 
