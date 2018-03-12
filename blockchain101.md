### block
> what is block? 
>>block is the basic element of blockchain a blockchain is built by a bunch of blocks.

> what is block made of ?
>> a block is made of. first a block should have a timestamp which indicate the block creating time . second the block should content a data . store the block meta info . third the block should store the hash of previous block , the last a block should have its own hash.
```go
type Block struct{
    Timestamp int64
    Hash []byte
    PreviousBlockHash []byte
    Data []byte
    
}
```

>how to create hash?
>> normally a hash is created by using hash argorithm . the seed of the hash normally using the block content which is timestamp ,data , and privous hash.

```go
func (b *Block) setHash(){
    timestamp :=[]byte(strconv.FormatInt(b.Timestamp,10))
    header := bytes.join([][]byte{b.PreviousBlockHash,b.Data,timestamp},[]byte{})
    b.hash = sha256.Sum256(header)[:]
}
```

> what is genesis block.
>> genesis block is the first block that is created when the blockchain goes alive
.it can be configurated by the blockchain creator.

```
func createGenesisBlock() *Block{
    return NewBlock('creating Genesis Block',[]byte{})
}

func NewBlock(data string, prevBlockHash []byte) *Block {
    b:=&Block{time.Now().Timestamp(),[]byte{},prevBlockHash,[]byte(data)}
    b.setHash()
    return b
}
```

### blockchain
> what is a blockchain.
>> blockchain is assembed by block. it is a chain of block . 

```go
type Blockchain struct {
    blocks []*Block
}
```

>how to create a blockchain?
>> create the genesis block . then the block chain is live .

```go
func createBlockChain() *Blockchain{
    return &Blockchain{[]Block{CreateGenesisBlock()}}
}
```

> what do you mean add a block to a blockchain?
>> In pow mining algorithem. when a miner calculated the right nounce then the miner become the author of a block ,which means the miner has the right to add the mined block to the blockchain. 
```go
func (bc *Blockchain) addBlock(data string){
    prevBlock := bc.blocks[bc.blocks.length -1]
    newBlock :=NewBlock(data,prevBlock.Hash)
    newBlock.setHash()
    append(bc.blocks,newBlock)
}
```





