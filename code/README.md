==========================================================================================
==========================================================================================
==========================================================================================
====                                                                                  ====
====                                   QRealFourier                                   ====
====                                                                                  ====
==========================================================================================
==========================================================================================
==========================================================================================


A Qt-based C++ library for Fast Fourier Transform (FFT) of real samples.

Last Release: 26 May 2012
Latest version: 0.2.0
Developer: Christoph Stallmann


==========================================================================================
=                                         About                                          =
==========================================================================================

QRealFourier is a Qt library for calculating FFTs on real samples. This library is based 
on FFTReal designed and created by Laurent de Soras. I take no credit and on the work done
in FFTReal. This purpose of this library is to create a simple-to-use interface for novice
FFT users in Qt. A single class (QFourierTransformer) provides a number of function for
handling forward and inverse FFTs and rescaling. A lot of the work is done during compile-
time which creates a more efficient FFT implementation. The library is slightly larger and
takes up a couple of extra bytes, but this is a fair trade-off for simplicity.

==========================================================================================
=                                     Requirements                                       =
==========================================================================================

QRealFourier requires CMake (2.6 or later) for makefile generation. Qt (4.7 or later) is
also required. Qt5 will be supported soon. Any native compilers should do, recommended are
g++ for Linux and MinGW for Windows.

==========================================================================================
=                                       Compiling                                        =
==========================================================================================

With Qt installed in the default location

1. mkdir build
2. cd build
3. cmake ..
5. make

With Qt installed in a custom location

1. mkdir build
2. cd build
3. ccmake ..
4. press 'c', 'c' and then 'g' (change Qt path if needed)
5. make

==========================================================================================
=                                         Usage                                          =
==========================================================================================

There are three examples under the example directory.
The first one illustrates a forward FFT, followed by an inverse FFT and the necessary
rescaling. The second one adds a Hamming window function to the execution. The last
example illustrates a FFT with a none-default size. Additionally the complex conjugate is
taken in the last example.
You can execute the examples by using the command:
./exampleN
where N indicates the number of the example (1, 2 or 3).

==========================================================================================
=                                      Fixed Sizes                                       =
==========================================================================================

QRealFourier is optimized for the following sizes:

8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192 and 16384

The transformation will have a better performance with chunks with one of these sizes.
Any other sizes will work, but pre-calculations have to be done during run-time which
reduces the performance.

==========================================================================================
=                                    Real & Imaginary                                    =
==========================================================================================

When doing the following:

transformer.forwardTransform(samples, fft);

the fft array will contain the following:

 Output array   | Values
 ---------------+----------------------
 f [0]          | Real (bin 0)
 f [...]        | Real (bin ...) 
 f [length/2]   | Real (bin length/2)
 f [length/2+1] | Imaginary (bin 1)
 f [...]        | Imaginary (bin ...)
 f [length-1]   | Imaginary (bin length/2-1)

The static function QFourierTransformer::toComplex(float input[])
can be used to convert this array into a QComplexVector. QComplexVector is a QVector
with QComplexNumbers, each number has a real and imaginary part.

==========================================================================================
=                                     The Main Class                                     =
==========================================================================================

QRealFourier is the interface that should be used for all transformations.

1.	QFourierTransformer(int size = -1);

	Constructor with optional parameter.

	The parameter specifies the fixed size of the FFT.
	The default is -1 which will result in a slower variable-sized FFT.
   
2.	Initialization setSize(int size);

	Changes the size to one of sizes given in the list above. This is the same as
	the parameter in the constructor. Can be changed during runtime.
   
	If the size is one of the default fixed sizes, FixedSize will be returned. If the
	size is not in the default list, but still a valid FFT size (power of 2),
	VariableSize will be returned. If the size is an invalid FFT size, InvalidSize is
	returned.
		
3.	void forwardTransform(float *input, float *output, QWindower *windower);

	Calculates the FFT of the input samples and stores them in output. The output array
	must be allocated beforehand and has the same size as the input array.

	If a window function is required, you can pass one to the function. The input array
	will automatically be adjusted according to the window function. If NULL is passed
	no windowing will be done.

4.	void inverseTransform(float input[], float output[]);

	Calculates the inverse FFT from the input and stores it as real samples in the output
	array. The output array must be allocated beforehand with the same size as input.
		
5.	void transform(float input[], float output[], QWindower *windower = NULL,
	Direction direction = QFourierTransformer::Forward);

	Calls one of the function in 3 or 4 above, depending on the direction parameter.
	This function is generally not recommended since it uses an extra if-statement.
	Rather use 3 or 4 instead.

6.	void rescale(float input[]);

	After the inverse FFT was calculated, you have to rescale the values of the samples.

7.	void conjugate(float input[]);

	Conjugates the imaginary part of the input. Hence the imaginary part's sign is changed
	Eg: input[length/2 + 1] = -input[length/2 + 1]

8.	QComplexVector toComplex(float input[]);

	Converts the FFT calculated with 4 or 5 above to a QVector of complex numbers
	as explained in the previous section.

==========================================================================================
=                                    Window Functions                                    =
==========================================================================================

The following window functions are supported:

1.	Rectangular
2.	Hamming
3.	Hann

==========================================================================================
=                                        History                                         =
==========================================================================================

*******************
** 26 May 2012   **
** Version 0.2.0 **
*******************
Entire restructuring of the library.
Separate running threads removed.
Conjugation support added.
Separate calculations (forward FFT, inverse FFT and rescaling) moved to the same class.
Variable and fixed FFTs now be done with the same functions.

*******************
** 22 May 2012   **
** Version 0.1.0 **
*******************
Initial release

==========================================================================================
=                                        Contact                                         =
==========================================================================================

QRealFourier is part of the Visore project:

http://www.visore.org
http://github.com/visore/Visore
http://sourceforge.net/projects/visore/

QRealFourier:

Christoph Stallmann
University of Pretoria
Department of Computer Science

visore.project@gmail.com
http://github.com/visore/QRealFourier
http://sourceforge.net/p/qrealfourier

FFTReal:

Laurent de Soras
laurent.de.soras@free.fr
http://ldesoras.free.fr
