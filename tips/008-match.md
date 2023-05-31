# The Match Statement
Python's new match statement (added in 3.10) allows to match structural patterns in an object. No new functionality is introduced, instead it is syntactic sugar for matching patterns without using many if-statements.

My favorite example of this would be a simple solution to FizzBuzz:
```python
for number in range(1_000):
    match (number % 5, number % 3):
        case (0, 0):
            print("FizzBuzz")
        case (0, _):
            print("Fizz")
        case (_, 0):
            print("Buzz")
        case (_, _):
            print(number)
```

Another example could be processing what to do after parsing command line arguments:
```python
match args.command, args.file:

    # Open a compressed file
    case Commands.OPEN, CompressedImage:
        open_file(decompress_file(args.file))

    # Open a decompressed file
    case Commands.OPEN, DecompressedImage:
        open_file(args.file)

    # Compress a decompressed file
    case Commands.COMPRESS, DecompressedImage:
        compress_file(args.file)

    # Decompress a compressed file
    case Commands.DECOMPRESS, CompressedImage:
        decompress_file(args.file)

    # Fail to de/compress any other file
    case Commands.COMPRESS, _:
        raise ValueError("Cannot compress this file")
    case Commands.DECOMPRESS, _:
        raise ValueError("Cannot decompress this file")
```

There is more to learn about structural pattern matching in the references below.

References:
- https://peps.python.org/pep-0634/
- https://peps.python.org/pep-0636/
