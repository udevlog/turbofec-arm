About TurboFEC-ARM
==================

TurboFEC-ARM is an adaptation of the [TurboFEC library](https://github.com/ttsou/turbofec), originally designed for x86 architectures, now ported to support ARM-based systems such as Raspberry Pi 5 and similar ARM devices. This library includes implementations of LTE forward error correction encoders and decoders, covering convolutional codes, turbo codes, and the associated rate matching units that manage block interleaving, bit selection, and pruning.

The ARM version leverages NEON instructions for optimized performance on ARM hardware.

LTE specification and sections:

3GPP TS 36.212 *"LTE Multiplexing and channel coding"*

5.1.3.1 *"Tail biting convolutional code"*

5.1.3.2 *"Turbo encoding"*

5.1.3.1 *"Rate matching for turbo coded transport channels"*

5.1.3.2 *"Rate matching for convolutionally coded transport channels and control information"*

Compile on ARM or Docker Container
==================================
**- To build on a Raspberry Pi or similar ARM device:**
```sh
$ autoreconf -i
$ ./configure
$ make
$ sudo make install
```

**- To build in a Docker container:**

Ensure you have Docker installed on your system. For detailed instructions on installing Docker, please refer to the 'Docker Installation' section at the bottom of this document.

1. **Pull the Docker image**:
   This image includes the necessary ARM emulation environment using QEMU.
   ```sh
   $ docker pull udevlog/turbofec-arm
   ```

2. **Start the Docker container**:
   Launch the Docker container with QEMU for ARM emulation.
   ```sh
   $ docker run -it --rm udevlog/turbofec-arm
   ```

3. **Inside the Docker container**:
   Execute the following commands to build the project.
   ```sh
   $ autoreconf -i
   $ ./configure --host=aarch64-linux-gnu
   $ make
   $ make install
   ```

   This setup will compile and install the TurboFEC library and its dependencies within the container. Note that any changes inside the container will be lost unless you commit them to a new Docker image.


Testing on Raspberry Pi
=======================
Performance on Raspberry Pi is indicative of actual deployment conditions and typically showcases much higher processing rates compared to the emulated environment.


1. **Build test tools**:
   ```sh
   make -C ./tests turbo_test conv_test
   ```

2. **Run all automated tests**:
   This will execute all configured tests and output results.
   ```sh
   make check
   ```

3. **Benchmark specific code**:
   Here is how to perform a benchmark test on the 3GPP LTE turbo encoder and decoder:
    ```sh
    $ tests/.libs/turbo_test -b -j 4 -i 1 -p 10000

    =================================================
    [+] Testing: 3GPP LTE turbo
    [.] Specs: (N=2, K=4), Length 6144

    [.] Performance benchmark:
    [..] Decoding 40000 bursts on 4 thread(s) with 1 iteration(s)
    [..] Testing:
    [..] Elapsed time....................... 2.855458 secs
    [..] Rate............................... 86.066754 Mbps
    ```

   This example demonstrates the potential performance capabilities of TurboFEC-ARM when running on native ARM hardware without the limitations of emulation.

4. **Run specific tests**:
   Individual tests and benchmarks can also be run to evaluate specific functionalities or performance aspects:
   ```sh
   tests/.libs/conv_test -h # Displays help for convolutional tests
   tests/.libs/turbo_test -h  # Displays help for turbo tests
   ```


Testing in Docker (Using QEMU for ARM Emulation)
================================================
Note: Performance metrics observed within the Docker container are expected to be lower due to the overhead of QEMU emulation, which does not have access to the full resources of the host machine.

1. **Build test tools**:
   ```sh
   make -C ./tests turbo_test conv_test
   ```

2. **List available codes and their specifications**:
   ```sh
   qemu-aarch64-static tests/.libs/conv_test -l
   ```

3. **Run specific tests**:
   For example, to test the WiMax FCH code:
   ```sh
   qemu-aarch64-static tests/.libs/conv_test -c 19
   ```

4. **Benchmarking**:
   Testing the performance of the 3GPP LTE turbo encoder and decoder:
   ```sh
   qemu-aarch64-static tests/.libs/turbo_test -b -j 8 -i 2 -p 1000
   ```

   This command tests the performance of the decoder using 8 threads, with each thread performing 2 iterations over 1000 packets, reflecting how QEMU can significantly reduce performance due to emulation.

5. **Run specific tests**:
   Individual tests and benchmarks can also be run to evaluate specific functionalities or performance aspects:
   ```sh
   qemu-aarch64-static tests/.libs/conv_test -h # Displays help for convolutional tests
   qemu-aarch64-static tests/.libs/turbo_test -h  # Displays help for turbo tests
   ```


Docker Installation
===================
**Docker**: Install Docker for [Windows 10](https://docs.docker.com/docker-for-windows/install/) or [Ubuntu](https://docs.docker.com/engine/install/ubuntu/) to run the ARM emulation environment.

Benchmark
=========
You can perform various convolutional and turbo decoding tests to assess performance on your ARM device or within the Docker emulation:

```sh
$ tests/.libs/turbo_test -b -j 4 -i 1 -p 10000

=================================================
[+] Testing: 3GPP LTE turbo
[.] Specs: (N=2, K=4), Length 6144

[.] Performance benchmark:
[..] Decoding 40000 bursts on 4 thread(s) with 1 iteration(s)
[..] Testing:
[..] Elapsed time....................... 2.855458 secs
[..] Rate............................... 86.066754 Mbps

```

Credits
=======
All credit for the original x86-based TurboFEC code goes to the original repository at: [TurboFEC](https://github.com/ttsou/turbofec).

Additionally, this ARM NEON adaptation utilizes the 'sse2neon' library to convert SSE intrinsics to their NEON counterparts. 'sse2neon' is a critical component in enabling the TurboFEC code to run on ARM architectures. More details and the source code can be found at [sse2neon](https://github.com/DLTcollab/sse2neon).
