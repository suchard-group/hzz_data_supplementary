This repository contains data files (xml) and instructions to reproduce the results in the manuscript "Hamiltonian zigzag accelerates large-scale inference for conditional dependencies between complex biological traits" by Zhenyu Zhang et al.
### (Optional) BEAGLE 
BEAGLE speeds up the tree inference in our applications. Please follow [BEAGLE installation instructions](https://github.com/beagle-dev/beagle-lib) to install BEAGLE.
### (Optional) Vectorized c++ code 
To accelerate the parallelizable operations in Zigzag-HMC, we write vectorized code in c++. The following commands would build a library: `zigzag/build/libzig_zag.dylib` that BEAST calls via the Java Native Interface.
```
git clone https://github.com/suchard-group/zigzag
cd zigzag
mkdir build
cmake ..
cmake ..
make
```
BEAGLE and vectorized c++ code both reduce the run-time but do not change any inference results. If you are not using them, omit the `-Djava.library.path` part in the reproducing commands.
### Setting up BEAST
We implement the Zigzag-HMC method in the `hmc_develop` branch of [BEAST](https://beast.community/). The following commands compile the source code. You may need to install ant by `brew install ant` through [Homebrew](https://brew.sh/) (Mac) or `sudo apt-get install ant` (Linux).
```
git clone -b hmc_develop https://github.com/beast-dev/beast-mcmc.git
cd beast-mcmc
ant
```
Then under `beast-mcmc/build/dist/` you can find the `jar` files: `beast.jar`, `beauti.jar` and `trace.jar`.
### Reproducing the analyses
You may use the following commands for the applications (HIV-1, H1, N1, _Aquilegia_ flower) as described in the manuscript. The output log files with ''corr'' and ''latent'' in their file names contain the MCMC samples for across-trait covariance elements and latent parameters, respectively. Columns "varianceMatrix" and "precisionMatrix"  
* HIV-1
	```
    java -Xmx10000m -Djava.library.path=/usr/local/lib:where_zigzag_is_located/build -jar where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_order 1 -seed 666 -overwrite where_this_repository_is_stored/xml/HIV/hiv.xml
    ```
* H1
	```
    java -Xmx10000m -Djava.library.path=/usr/local/lib:where_zigzag_is_located/build -jar where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_order 1 -seed 666 -overwrite where_this_repository_is_stored/xml/H1H1/H1.xml
    ```
* N1
	```
    java -Xmx10000m -Djava.library.path=/usr/local/lib:where_zigzag_is_located/build -jar where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -beagle_order 1 -seed 666 -overwrite where_this_repository_is_stored/xml/H1N1/N1.xml
    ```
* Aquilegia flower
	```
    java -jar where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -seed 666 -overwrite where_this_repository_is_stored/xml/flower/aquilegia.xml
    ```
* To reproduce the efficiency comparison results in Section 3.2: replace `xml_file_name` with one of `24t_BPS`, `24t_HZZ`, `24t_LGHMC`, `24t_ZNUTS`.  
	```
    java -jar where_beast_is_git_cloned/beast-mcmc/build/dist/beast.jar -seed 666 -overwrite where_this_repository_is_stored/xml/comparison/xml_file_name.xml
    ```
