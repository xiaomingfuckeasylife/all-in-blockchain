## the concept proof of work 

> what is proof of work ?
>> A proof of work is a mechanism that used to proof that a node has done the work(has the right) to gain a reward from the blockchain it makes the blockchain more secure for not anyone can put a block into the blockchain , the 'work' normally will take a huge a mount of computation power.we can explain it in a simple way . in everyday job we have to work to sustain our life , the time we take , the work we done is the proof of work . our boss pay us money accounding to the proof of work. 
another thing about pow is that solve the problem is hard but verify it is easy , for if you say you calculate the right nonce , and broadcast to other node ,other node will check if your work actually right . 

> what is the work we need to do in blockchain?
>>before we explain that we sould talk about `hash` , what is `hash`?
>>>hash function is normally used to check the consistance of data . in blockchain a hash is used to guarantee the consistency of a block , the input data for calculate a hash contains the privous block hash , thus make it impossible to modify a block in the chain , one has to recalculate its hash and hashed of all the blockes after it .

>> it is a math problem . that everyone has to try from time to time , until they get it right . it is like a brute force to get the password right. in blockchain we call it nonce . our work is calculate the right nonce that matches the problem . 
>>> 1. take block header data . 
>>> 2. add a counter to it . the counter start at 0 
>>> 3. get a hash of the `data + counter` combination .
>>> 4. check if the hash meets the requirement . if not restart from 2 ,if yes .break out the loop.

## implementation 
```go
constant targetBits = 24 . 
```
> the block mining difficulty . for simplicity we will using a global constant variable . but in bitcoin the block creation time is 10 minutes so the mining difficulty is changed accourding to the globla mining power . 

```go
type ProofOfWork struct {
    block  *Block
    target *Big.int
}

func NewProofOfWork(b *Block) *ProofOfWork{
    target := big.NewInt(1)
    target.Lsh(target,uint(256-targetBits))
    return &ProofOfWork{b,target}
}
```
> i use a proofOfwork struct to hold the block and its target , the target is used to compare the computed hash value. the target is calculated by left shift 1 to a certain number. 

```go
func (pow *ProofOfWork) prepareData(nonce int) []byte{
    
    data := bytes.join([][]byte(pow.block.data,pow.block.prevBlockHash,IntToHex(pow.block.timestamp),IntToHex(int64(targetBits)),IntToHex(int64(nonce))),[]byte{})
    
    return data
}
```
> prepare the computated data which will be used to compare to the target hash value . if the data is valid the data will be used to calculate current block hash.

```go
func (pow *ProofOfWork) run() (int , []byte){
    var hashInt big.Int
    var hash [32]byte
    nonce := 0
    fmt.Printf("Mining the block containing \"%s\" \n",pow.block.Data)
    for nonce < math.MaxInt64 {
        data := pow.prepareData(nonce)
        hash = sha256.Sum256(data)
        fmt.Printf("\r%x",hash)
        hashInt.setBytes(hash[:])
        if hashInt.Cmp(pow.target) == -1{
            break 
        }else {
            nonce++
        }
    }
    return nonce , hash[:]
}
```
> calculate the rightful nonce . 

```go
func (pow *ProofOfwork) validate() bool{
    var hashInt big.Int 
    data := pow.prepareData(pow.block.nonce)
    hash := sha256.Sum256(data)
    hashInt.SetBytes(hash[:])
    return hashInt.Cmp(pow.target) == -1
}

type Block struct{
    ...
    nonce int64
}

```
> validate if the nonce is right .

```go
func NewBlock(data string, prevBlockHash []byte) *Block{
    block := &Block{time.now().Unix(),[]byte(data),prevBlockHash,0}
    pow := NewProofOfWork(block)
    nonce , hash := pow.Run()
    block.Hash = hash
    block.Nonce = nonce
    return block
}
```

>now if we create a new block we have to mining it first calculate the rightful nonce and hash . then we can add the blockchain . 
