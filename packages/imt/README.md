<p align="center">
    <h1 align="center">
        Incremental Merkle Trees
    </h1>
    <p align="center">Incremental Merkle tree implementations in TypeScript.</p>
</p>

<p align="center">
    <a href="https://github.com/privacy-scaling-explorations/zk-kit">
        <img src="https://img.shields.io/badge/project-zk--kit-blue.svg?style=flat-square">
    </a>
    <a href="https://github.com/privacy-scaling-explorations/zk-kit/tree/main/packages/imt/LICENSE">
        <img alt="NPM license" src="https://img.shields.io/npm/l/%40zk-kit%2Fimt?style=flat-square">
    </a>
    <a href="https://www.npmjs.com/package/@zk-kit/imt">
        <img alt="NPM version" src="https://img.shields.io/npm/v/@zk-kit/imt?style=flat-square" />
    </a>
    <a href="https://npmjs.org/package/@zk-kit/imt">
        <img alt="Downloads" src="https://img.shields.io/npm/dm/@zk-kit/imt.svg?style=flat-square" />
    </a>
    <a href="https://bundlephobia.com/package/@zk-kit/imt">
        <img alt="npm bundle size (scoped)" src="https://img.shields.io/bundlephobia/minzip/@zk-kit/imt" />
    </a>
    <a href="https://eslint.org/">
        <img alt="Linter eslint" src="https://img.shields.io/badge/linter-eslint-8080f2?style=flat-square&logo=eslint" />
    </a>
    <a href="https://prettier.io/">
        <img alt="Code style prettier" src="https://img.shields.io/badge/code%20style-prettier-f8bc45?style=flat-square&logo=prettier" />
    </a>
</p>

<div align="center">
    <h4>
        <a href="https://appliedzkp.org/discord">
            🗣️ Chat &amp; Support
        </a>
        <span>&nbsp;&nbsp;|&nbsp;&nbsp;</span>
        <a href="https://zkkit.pse.dev/modules/_zk_kit_imt.html">
            📘 Docs
        </a>
    </h4>
</div>

An Incremental Merkle Tree is a dynamic data structure optimized for verifying data integrity in growing datasets. The updates are efficient since they modify only relevant nodes, avoiding the need to rebuild the entire tree with each change. This makes it ideal for real-time data verification contexts. You can learn more about the differences between MT and IMT [here](https://zokyo-auditing-tutorials.gitbook.io/zokyo-tutorials/tutorial-16-zero-knowledge-zk/definitions-and-essentials/incremental-merkle-tree).

> [!WARNING]  
> If you are looking for the first version of this package, please visit this [link](https://github.com/privacy-scaling-explorations/zk-kit/tree/imt-v1/packages/incremental-merkle-tree).

---

## 🛠 Install

### npm or yarn

Install the `@zk-kit/imt` package with npm:

```bash
npm i @zk-kit/imt --save
```

or yarn:

```bash
yarn add @zk-kit/imt
```

### CDN

You can also load it using a `script` tag using [unpkg](https://unpkg.com/):

```html
<script src="https://unpkg.com/@zk-kit/imt"></script>
```

or [JSDelivr](https://www.jsdelivr.com/):

```html
<script src="https://cdn.jsdelivr.net/npm/@zk-kit/imt"></script>
```

## 📜 Usage

### IMT

In this implementation, the tree is built with a predetermined depth, utilizing a list of zeros (one for each level) to hash nodes lacking fully defined children. The tree's branching factor, or the number of children per node, can be customized via the arity parameter. For detailed insights into the implementation specifics, please refer to the [technical documentation](https://zkkit.pse.dev/classes/_zk_kit_imt.IMT.html).

```typescript
import { IMT } from "@zk-kit/imt"
import { poseidon2 } from "poseidon-lite"

/**
 * depth: number of nodes from the leaf to the tree's root node.
 * zeroValue: default zero, can vary based on the specific use-case.
 * arity: number of children per node (2 = Binary IMT, 5 = Quinary IMT).
 */

const depth = 16
const zeroValue = 0
const arity = 2

/**
 * To create an instance of an IMT, you need to provide the hash function
 * used to compute the tree nodes, as well as the depth, zeroValue, and arity of the tree.
 */
const tree = new IMT(poseidon2, depth, zeroValue, arity)

// You can also initialize a tree with a given list of leaves.
// const leaves = [1, 2, 3]
// new IMT(poseidon2, depth, zeroValue, arity, leaves)

// Insert (incrementally) a leaf with a value of 1.
tree.insert(1)

// Insert (incrementally) a leaf with a value of 3.
tree.insert(3)

// 6176938709541216276071057251289703345736952331798983957780950682673395007393n.
console.log(tree.root)
/*
[
  0,
  14744269619966411208579211824598458697587494354926760081771325075741142829156n,
  7423237065226347324353380772367382631490014989348495481811164164159255474657n,
  11286972368698509976183087595462810875513684078608517520839298933882497716792n,
  3607627140608796879659380071776844901612302623152076817094415224584923813162n,
  19712377064642672829441595136074946683621277828620209496774504837737984048981n,
  20775607673010627194014556968476266066927294572720319469184847051418138353016n,
  3396914609616007258851405644437304192397291162432396347162513310381425243293n,
  21551820661461729022865262380882070649935529853313286572328683688269863701601n,
  6573136701248752079028194407151022595060682063033565181951145966236778420039n,
  12413880268183407374852357075976609371175688755676981206018884971008854919922n,
  14271763308400718165336499097156975241954733520325982997864342600795471836726n,
  20066985985293572387227381049700832219069292839614107140851619262827735677018n,
  9394776414966240069580838672673694685292165040808226440647796406499139370960n,
  11331146992410411304059858900317123658895005918277453009197229807340014528524n,
  15819538789928229930262697811477882737253464456578333862691129291651619515538n
]
*/
console.log(tree.zeroes)
// 2
console.log(tree.arity)
// 16
console.log(tree.depth)
// [1, 3]
console.log(tree.leaves)

// Get the index of the leaf with value 3.
const idx = tree.indexOf(3)
// 1
console.log(idx)

// Update the value of the leaf at position 1 to 2.
tree.update(1, 2)
// [1, 2]
console.log(tree.leaves)

// Delete leaf at position 1.
tree.delete(1)
// [1, 0]
console.log(tree.leaves)

/**
 * Compute a Merkle Inclusion Proof (proof of membership) for the leaf with index 1.
 * The proof is only valid if the value 1 is found in a leaf of the tree.
 */
const proof = tree.createProof(1)
// true
console.log(tree.verifyProof(proof))
```

### Lean IMT

The LeanIMT is an optimized binary version of the IMT into binary-focused model, eliminating the need for zero values and allowing dynamic depth adjustment. Unlike the IMT, which uses a zero hash for incomplete nodes, the LeanIMT directly adopts the left child's value when a node lacks a right counterpart. The tree's depth dynamically adjusts to the count of leaves, enhancing efficiency by reducing the number of required hash calculations. For detailed insights into the implementation specifics, please refer to the [technical documentation](https://zkkit.pse.dev/classes/_zk_kit_imt.LeanIMT.html).

```typescript
import { LeanIMT } from "@zk-kit/imt"
import { poseidon2 } from "poseidon-lite"

/**
 * Hash function used to compute the tree nodes.
 */
const hash = (a, b) => poseidon2([a, b])

// To create an instance of a LeanIMT, you must provide the hash function.
const tree = new LeanIMT(hash)

// You can also initialize a tree with a given list of leaves.
// const leaves = [1n, 2n, 3n]
// new LeanIMT(hash, leaves)

// LeanIMT is strictly typed. Default type for nodes is 'bigint',
// but you can set your own type.
// new LeanIMT<number>((a, b) => a + b)

// Insert (incrementally) a leaf with a value of 1.
tree.insert(1n)
// [1n]
console.log(tree.leaves)

// Insert (incrementally) a leaf with a value of 3.
tree.insert(3n)

// 21106761926285267690763443010820487107972411248208546226053195422384279971821n
console.log(tree.root)
// 1
console.log(tree.depth)
// 2
console.log(tree.size)
// [1n, 3n]
console.log(tree.leaves)

// Get the index of the leaf with value 3n.
const idx = tree.indexOf(3n)
// 1
console.log(idx)

// Check if the tree contains a leaf with value 4n.
const has = tree.has(4n)
// false
console.log(tree.has(4n))

// Update the value of the leaf at position 1 to 2n.
tree.update(1, 2n)
// [1n, 2n]
console.log(tree.leaves)

/**
 * If you want to delete a leaf with LeanIMT you can use the update function with an
 * arbitrary value to be used for the removed leaves.
 */

// Update the value of the leaf at position 1 to 0n (deletion).
tree.update(1, 0n)
// [1n, 0n]
console.log(tree.leaves)

/**
 * Compute a Merkle Inclusion Proof (proof of membership) for the leaf with index 1.
 * The proof is only valid if the value 1 is found in a leaf of the tree.
 */
const proof = tree.generateProof(1)
// true
console.log(tree.verifyProof(proof))
```
