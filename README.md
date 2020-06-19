# intel-mkl-src

|crate | crate.io | description |
|:-----|:--------:|:------------|
|intel-mkl-src| [![Crate](http://meritbadge.herokuapp.com/intel-mkl-src)](https://crates.io/crates/intel-mkl-src)| Source crate for Intel-MKL |
|intel-mkl-sys| [![Crate](http://meritbadge.herokuapp.com/intel-mkl-sys)](https://crates.io/crates/intel-mkl-sys)| FFI for Intel-MKL [vector math][VM], and [statistical functions][VSL] |
|intel-mkl-tool| [![Crate](http://meritbadge.herokuapp.com/intel-mkl-tool)](https://crates.io/crates/intel-mkl-tool)| CLI utility for redistributing Intel-MKL |

Redistribution of Intel MKL as a crate. Tested on Linux, macOS, and Windows (since 0.4.0)

[VM]:  https://software.intel.com/en-us/mkl-developer-reference-c-vector-mathematical-functions
[VSL]: https://software.intel.com/en-us/mkl-developer-reference-c-statistical-functions

## Supported features

| feature name           | Linux              | macOS              | Windows            |
|:-----------------------|:------------------:|:------------------:|:------------------:|
| mkl-static-lp64-iomp   | :heavy_check_mark: | -                  | -                  |
| mkl-static-lp64-seq    | :heavy_check_mark: | -                  | -                  |
| mkl-static-ilp64-iomp  | :heavy_check_mark: | -                  | -                  |
| mkl-static-ilp64-seq   | :heavy_check_mark: | -                  | -                  |
| mkl-dynamic-lp64-iomp  | :heavy_check_mark: | :heavy_check_mark: | -                  |
| mkl-dynamic-lp64-seq   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| mkl-dynamic-ilp64-iomp | :heavy_check_mark: | :heavy_check_mark: | -                  |
| mkl-dynamic-ilp64-seq  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## Usage

This crate is a `*-src` crate. This downloads and link Intel MKL, but does not introduce any symbols.
Please use `blas-sys`, `lapack-sys`, or `fftw-sys` to use BLAS, LAPACK, FFTW interface of MKL, e.g.

```toml
[dependencies]
fftw-sys = { version = "0.4", features = ["intel-mkl"] }
```

## Find system MKL libraries

This crate will download archive from AWS S3 `rust-intel-mkl` bucket if no MKL library installed by other way, e.g. [apt]/[yum].
`intel-mkl-src` seeks MKL in following manner:

- Check `${OUT_DIR}` where previous build has downloaded
- Seek using [pkg-config] crate
  - `${PKG_CONFIG_PATH}` has to be set correctly. It may not be set by default in usual install.
  - You can confirm it by checking the following command returns error.
    ```
    pkg-config --libs mkl-dynamic-lp64-iomp
    ```
- (experimental) Seek `${XDG_DATA_HOME}/intel-mkl-tool`
- Seek a directory set by `${MKLROOT}` environment variable
- Seek `/opt/intel/mkl`

[apt]: https://software.intel.com/content/www/us/en/develop/articles/installing-intel-free-libs-and-python-apt-repo.html
[yum]: https://software.intel.com/content/www/us/en/develop/articles/installing-intel-free-libs-and-python-yum-repo.html
[pkg-config]: https://github.com/rust-lang/pkg-config-rs

## License
MKL is distributed under the Intel Simplified Software License for Intel(R) Math Kernel Library, See [License.txt](License.txt).
Some wrapper codes are licensed by MIT License (see the header of each file).
