# Nifti.NET

## Test Results: Double speed

using free unity volume rendering in asset store the niftii loading for  a simple mri reduced reading time from 800ms to 400ms!


## Optimizations Made
### Read All Data at Once:

Original Code: The original method reads each element (e.g., each float, int, short) one at a time in a loop, which requires repeated Read operations on the stream.
Optimized Code: The optimized method reads all the data at once into a byte array (byteArray), minimizing the number of I/O operations. This reduces the time spent waiting on disk or memory reads, as a single Read operation is generally much faster than thousands of small Read operations.

### Use of MemoryStream and BinaryReader:

Original Code: Reads each element directly from the stream, requiring individual conversions and endianness handling.
Optimized Code: Wraps the byte array in a MemoryStream and a BinaryReader. This approach allows us to process the byte array entirely in memory and handle endianness and conversions (e.g., ReadInt32, ReadSingle) without additional complexity. The BinaryReader provides efficient methods for reading common data types directly, which improves readability and potentially improves performance.

### Handling Endianness Efficiently:

Original Code: Each call to ReadFloat, ReadInt, etc., requires checking endianness for every single value.
Optimized Code: Endianness is handled using the ReverseBytes helper methods only if necessary. Since the entire data is in memory, reversing bytes (if required) can be done quickly in bulk operations without additional stream reads.

### Reduced Method Call Overhead:

Original Code: Repeatedly calls custom methods (ReadFloat, ReadInt, etc.), which increases method call overhead.
Optimized Code: Uses BinaryReader methods to perform reads directly, minimizing function calls and allowing direct conversion from bytes to the target data types.

If you need  any questions regardin the optimisations made feel free to email me at: farhad.peeri@gmail.com

## Why These Changes Increase Speed

The main reason for the increased speed is the reduction in I/O operations. File and stream I/O are typically much slower than in-memory operations. By reading all data into memory at once and then processing it with efficient in-memory operations, we significantly reduce the overall time spent waiting on I/O.

Additionally, working with BinaryReader and MemoryStream allows for batch processing and avoids repeated function calls, further optimizing the data processing. The use of MemoryStream and BinaryReader for in-memory parsing is generally faster than individually reading and converting each element from the original stream.

These changes result in a faster data read process, especially for large datasets, because we leverage the faster memory operations over slow I/O operations.

Original repo is at :
forked from https://github.com/plwp/Nifti.NET

A basic library for reading, writing and manipulating NIfTI files.

(If you're looking for the TensorFlow CNN platform, try NiftyNet (https://niftynet.io/))
