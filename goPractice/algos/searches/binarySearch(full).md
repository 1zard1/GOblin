```go
package main

const INF = int(1e9 + 7)

func LowerBound(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] < key {
			lo = mi + 1
		} else {
			ans = mi
			hi = mi - 1
		}
	}
	return ans
}

func LowerBoundRecHelp(nums []int, key int, lo int, hi int) int {
	if lo > hi {
		return INF
	}
	mi := lo + (hi-lo)/2
	if nums[mi] < key {
		return LowerBoundRecHelp(nums, key, mi+1, hi)
	}
	return min(mi, LowerBoundRecHelp(nums, key, lo, mi-1))
}

func LowerBoundRec(nums []int, key int) int {
	return LowerBoundRecHelp(nums, key, 0, len(nums)-1)
}

func UpperBound(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] <= key {
			lo = mi + 1
		} else {
			ans = mi
			hi = mi - 1
		}
	}
	return ans
}

func UpperBoundRecHelp(nums []int, key int, lo int, hi int) int {
	if lo > hi {
		return INF
	}
	mi := lo + (hi-lo)/2
	if nums[mi] <= key {
		return UpperBoundRecHelp(nums, key, mi+1, hi)
	}
	return min(mi, UpperBoundRecHelp(nums, key, lo, mi-1))
}

func UpperBoundRec(nums []int, key int) int {
	return UpperBoundRecHelp(nums, key, 0, len(nums)-1)
}

func BinarySearch(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] == key {
			return mi
		} else if nums[mi] < key {
			lo = mi + 1
		} else {
			hi = mi - 1
		}
	}
	return -1
}

func BinarySearchRec(nums []int, key int, lo int, hi int) int {
	if lo > hi {
		return -1
	}
	mi := lo + (hi-lo)/2
	if nums[mi] == key {
		return mi
	}
	if nums[mi] < key {
		return BinarySearchRec(nums, key, mi+1, hi)
	}
	return BinarySearchRec(nums, key, lo, mi-1)
}

func BinaryFirst(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] < key {
			lo = mi + 1
		} else if nums[mi] == key {
			ans = mi
			hi = mi - 1
		} else {
			hi = mi - 1
		}
	}
	return ans
}

func BinaryFirstRec(nums []int, key int, lo int, hi int) int {
	if lo > hi {
		return INF
	}
	mi := lo + (hi-lo)/2
	if nums[mi] < key {
		return BinaryFirstRec(nums, key, mi+1, hi)
	}
	if nums[mi] > key {
		return BinaryFirstRec(nums, key, lo, mi-1)
	}
	return min(mi, BinaryFirstRec(nums, key, lo, mi-1))
}

func BinaryLast(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] < key {
			lo = mi + 1
		} else if nums[mi] == key {
			ans = mi
			lo = mi + 1
		} else {
			hi = mi - 1
		}
	}
	return ans
}

func BinaryLastRec(nums []int, key int, lo int, hi int) int {
	if lo > hi {
		return -1
	}
	mi := lo + (hi-lo)/2
	if nums[mi] < key {
		return BinaryLastRec(nums, key, mi+1, hi)
	}
	if nums[mi] > key {
		return BinaryLastRec(nums, key, lo, mi-1)
	}
	return max(mi, BinaryLastRec(nums, key, mi+1, hi))
}

func Range(nums []int, key int) []int {
	left := BinaryFirst(nums, key)
	right := BinaryLast(nums, key)
	return []int{left, right}
}

func RangeRec(nums []int, key int) []int {
	left := BinaryFirstRec(nums, key, 0, len(nums)-1)
	right := BinaryLastRec(nums, key, 0, len(nums)-1)
	return []int{left, right}
}

func BinaryFirstLess(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] < key {
			ans = mi
			lo = mi + 1
		} else if nums[mi] == key {
			hi = mi - 1
		} else {
			hi = mi - 1
		}
	}
	return ans
}

func BinaryFirstGreater(nums []int, key int) int {
	lo := 0
	hi := len(nums) - 1
	ans := -1
	for lo <= hi {
		mi := lo + (hi-lo)/2
		if nums[mi] < key {
			lo = mi + 1
		} else if nums[mi] == key {
			lo = mi + 1
		} else {
			ans = mi
			hi = mi - 1
		}
	}
	return ans
}

func RangeLtGt(nums []int, key int) []int {
	left := BinaryFirstLess(nums, key)
	right := BinaryFirstGreater(nums, key)
	return []int{left, right}
}

func main() {
	Assert := func(ok bool) {
		if !ok {
			panic("Assertion is failed")
		}
	}

	AreEqual := func(L1, L2 []int) bool {
		M := len(L1)
		N := len(L2)
		if M != N {
			return false
		}
		for i := 0; i < N; i++ {
			if L1[i] != L2[i] {
				return false
			}
		}
		return true
	}

	Assert(LowerBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 1) == 0)
	Assert(LowerBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 0) == 0)
	Assert(LowerBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10) == -1)
	Assert(LowerBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4) == 3)
	Assert(LowerBound([]int{1, 2, 3, 4, 6, 7, 8, 9}, 5) == 4)
	Assert(LowerBound([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 3) == 2)
	Assert(LowerBound([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 4) == 7)
	Assert(LowerBound([]int{1, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 2) == 1)

	Assert(LowerBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 1) == 0)
	Assert(LowerBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 0) == 0)
	Assert(LowerBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10) == INF)
	Assert(LowerBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4) == 3)
	Assert(LowerBoundRec([]int{1, 2, 3, 4, 6, 7, 8, 9}, 5) == 4)
	Assert(LowerBoundRec([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 3) == 2)
	Assert(LowerBoundRec([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 4) == 7)
	Assert(LowerBoundRec([]int{1, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 2) == 1)

	Assert(UpperBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4) == 4)
	Assert(UpperBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 8) == 8)
	Assert(UpperBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10) == -1)
	Assert(UpperBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 0) == 0)
	Assert(UpperBound([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 9) == -1)
	Assert(UpperBound([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 3) == 7)

	Assert(UpperBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4) == 4)
	Assert(UpperBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 8) == 8)
	Assert(UpperBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10) == INF)
	Assert(UpperBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 0) == 0)
	Assert(UpperBoundRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 9) == INF)
	Assert(UpperBoundRec([]int{1, 2, 3, 3, 3, 3, 3, 4, 6, 7, 8, 9}, 3) == 7)

	Assert(BinarySearch([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4) == 3)
	Assert(BinarySearch([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10) == -1)

	Assert(BinarySearchRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 4, 0, 8) == 3)
	Assert(BinarySearchRec([]int{1, 2, 3, 4, 5, 6, 7, 8, 9}, 10, 0, 8) == -1)

	Assert(AreEqual(Range([]int{1, 2, 8, 8, 8, 10, 10}, 8), []int{2, 4}))
	Assert(AreEqual(Range([]int{1, 2, 8, 8, 10, 19, 19, 19}, 19), []int{5, 7}))
	Assert(AreEqual(Range([]int{1, 1, 1, 2, 8, 8, 12, 19}, 1), []int{0, 2}))

	Assert(AreEqual(RangeRec([]int{1, 2, 8, 8, 8, 10, 10}, 8), []int{2, 4}))
	Assert(AreEqual(RangeRec([]int{1, 2, 8, 8, 10, 19, 19, 19}, 19), []int{5, 7}))
	Assert(AreEqual(RangeRec([]int{1, 1, 1, 2, 8, 8, 12, 19}, 1), []int{0, 2}))

	Assert(AreEqual(RangeLtGt([]int{1, 2, 8, 10, 10, 12, 19}, 0), []int{-1, 0}))
	Assert(AreEqual(RangeLtGt([]int{1, 2, 8, 10, 10, 12, 19}, 5), []int{1, 2}))
	Assert(AreEqual(RangeLtGt([]int{1, 2, 8, 10, 10, 12, 19}, 20), []int{6, -1}))
	Assert(AreEqual(RangeLtGt([]int{1, 2, 8, 12, 19}, 10), []int{2, 3}))
	Assert(AreEqual(RangeLtGt([]int{1, 2, 8, 10, 10, 12, 19}, 13), []int{5, 6}))
}
```
