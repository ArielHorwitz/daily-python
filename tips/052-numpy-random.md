# Numpy Random Generators

As one may expect, numpy has a (pseudo-) random number generator to generate random data samples. Most functionality is available using a generator produced by `np.random.default_rng()`:
```python
import numpy as np
rng = np.random.default_rng()

integers = rng.integers(0, 10, size=1_000_000)
assert integers.min() >= 0 and integers.max() <= 9

floats = rng.random(1_000_000)
assert floats.min() >= 0 and floats.max() < 1

percentages = np.linspace(0, 1, 100, endpoint=False)
waypoints = rng.choice(percentages, 10)
assert set(percentages) >= set(waypoints)
unique = np.unique(waypoints)
assert sorted(unique) == sorted(set(waypoints))
```

The "pseudo" in "pseudo-random" is due to the generator being deterministic. It starts with a *seed* that is provided to it and continuously derives new values from that seed. That means that the same output is always produced from the same input. So not really random, is it? But if the seed is random, then each successive value is essentially random.

Passing no arguments to `np.random.default_rng()` will generate some random seed for us, but we can specify a seed:
```python
SEEDED_INTEGERS = np.random.default_rng(42069).integers(10, size=1_000_000)
same_integers = np.random.default_rng(42069).integers(10, size=1_000_000)
assert np.array_equal(SEEDED_INTEGERS, same_integers)

new_integers = np.random.default_rng(420).integers(10, size=1_000_000)
assert not np.array_equal(SEEDED_INTEGERS, new_integers)
new_integers = np.random.default_rng(69).integers(10, size=1_000_000)
assert not np.array_equal(SEEDED_INTEGERS, new_integers)

# An actually secure PRNG:
import secrets
secure_rng = np.random.default_rng(secrets.randbits(256))
```

And of course there are many distributions to sample from (see documentation), here's a couple of examples:
```python
# Sample a binomial distribution
six_sixes = rng.binomial(6, 1/6, size=1_000_000)
lottery_winners = np.flatnonzero(six_sixes == 6)
assert 0.5 / 6**6 < len(lottery_winners) / 1_000_000 < 2 / 6**6

# Sample a normal distribution
normal = rng.normal(0, 3, size=1_000_000).astype(int)
densities, bins = np.histogram(normal, bins=15, range=(-7, 8), density=True)
assert densities[1] < densities[4] < densities[7]
assert densities[-1] < densities[-4] < densities[7]
```

References:
- https://numpy.org/doc/stable/reference/random/generator.html
- https://numpy.org/doc/stable/reference/random/index.html
