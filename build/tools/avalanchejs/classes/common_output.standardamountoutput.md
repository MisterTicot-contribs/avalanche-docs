[avalanche](../README.md) › [Common-Output](../modules/common_output.md) › [StandardAmountOutput](common_output.standardamountoutput.md)

# Class: StandardAmountOutput

An [Output](common_output.output.md) class which specifies a token amount .

## Hierarchy

  ↳ [Output](common_output.output.md)

  ↳ **StandardAmountOutput**

  ↳ [AmountOutput](api_avm_outputs.amountoutput.md)

  ↳ [AmountOutput](api_platformvm_outputs.amountoutput.md)

## Index

### Constructors

* [constructor](common_output.standardamountoutput.md#constructor)

### Properties

* [_typeID](common_output.standardamountoutput.md#protected-_typeid)
* [_typeName](common_output.standardamountoutput.md#protected-_typename)
* [addresses](common_output.standardamountoutput.md#protected-addresses)
* [amount](common_output.standardamountoutput.md#protected-amount)
* [amountValue](common_output.standardamountoutput.md#protected-amountvalue)
* [locktime](common_output.standardamountoutput.md#protected-locktime)
* [numaddrs](common_output.standardamountoutput.md#protected-numaddrs)
* [threshold](common_output.standardamountoutput.md#protected-threshold)

### Methods

* [clone](common_output.standardamountoutput.md#abstract-clone)
* [create](common_output.standardamountoutput.md#abstract-create)
* [deserialize](common_output.standardamountoutput.md#deserialize)
* [fromBuffer](common_output.standardamountoutput.md#frombuffer)
* [getAddress](common_output.standardamountoutput.md#getaddress)
* [getAddressIdx](common_output.standardamountoutput.md#getaddressidx)
* [getAddresses](common_output.standardamountoutput.md#getaddresses)
* [getAmount](common_output.standardamountoutput.md#getamount)
* [getLocktime](common_output.standardamountoutput.md#getlocktime)
* [getOutputID](common_output.standardamountoutput.md#abstract-getoutputid)
* [getSpenders](common_output.standardamountoutput.md#getspenders)
* [getThreshold](common_output.standardamountoutput.md#getthreshold)
* [getTypeID](common_output.standardamountoutput.md#gettypeid)
* [getTypeName](common_output.standardamountoutput.md#gettypename)
* [makeTransferable](common_output.standardamountoutput.md#abstract-maketransferable)
* [meetsThreshold](common_output.standardamountoutput.md#meetsthreshold)
* [select](common_output.standardamountoutput.md#abstract-select)
* [serialize](common_output.standardamountoutput.md#serialize)
* [toBuffer](common_output.standardamountoutput.md#tobuffer)
* [toString](common_output.standardamountoutput.md#tostring)
* [comparator](common_output.standardamountoutput.md#static-comparator)

## Constructors

###  constructor

\+ **new StandardAmountOutput**(`amount`: BN, `addresses`: Array‹Buffer›, `locktime`: BN, `threshold`: number): *[StandardAmountOutput](common_output.standardamountoutput.md)*

*Overrides [OutputOwners](common_output.outputowners.md).[constructor](common_output.outputowners.md#constructor)*

*Defined in [src/common/output.ts:472](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L472)*

A [StandardAmountOutput](common_output.standardamountoutput.md) class which issues a payment on an assetID.

**Parameters:**

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`amount` | BN | undefined | A [BN](https://github.com/indutny/bn.js/) representing the amount in the output |
`addresses` | Array‹Buffer› | undefined | An array of [Buffer](https://github.com/feross/buffer)s representing addresses |
`locktime` | BN | undefined | A [BN](https://github.com/indutny/bn.js/) representing the locktime |
`threshold` | number | undefined | A number representing the the threshold number of signers required to sign the transaction  |

**Returns:** *[StandardAmountOutput](common_output.standardamountoutput.md)*

## Properties

### `Protected` _typeID

• **_typeID**: *any* = undefined

*Overrides [Output](common_output.output.md).[_typeID](common_output.output.md#protected-_typeid)*

*Defined in [src/common/output.ts:430](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L430)*

___

### `Protected` _typeName

• **_typeName**: *string* = "StandardAmountOutput"

*Overrides [Output](common_output.output.md).[_typeName](common_output.output.md#protected-_typename)*

*Defined in [src/common/output.ts:429](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L429)*

___

### `Protected` addresses

• **addresses**: *Array‹[Address](common_output.address.md)›* = []

*Inherited from [OutputOwners](common_output.outputowners.md).[addresses](common_output.outputowners.md#protected-addresses)*

*Defined in [src/common/output.ts:120](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L120)*

___

### `Protected` amount

• **amount**: *Buffer* = Buffer.alloc(8)

*Defined in [src/common/output.ts:445](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L445)*

___

### `Protected` amountValue

• **amountValue**: *BN* = new BN(0)

*Defined in [src/common/output.ts:446](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L446)*

___

### `Protected` locktime

• **locktime**: *Buffer* = Buffer.alloc(8)

*Inherited from [OutputOwners](common_output.outputowners.md).[locktime](common_output.outputowners.md#protected-locktime)*

*Defined in [src/common/output.ts:117](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L117)*

___

### `Protected` numaddrs

• **numaddrs**: *Buffer* = Buffer.alloc(4)

*Inherited from [OutputOwners](common_output.outputowners.md).[numaddrs](common_output.outputowners.md#protected-numaddrs)*

*Defined in [src/common/output.ts:119](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L119)*

___

### `Protected` threshold

• **threshold**: *Buffer* = Buffer.alloc(4)

*Inherited from [OutputOwners](common_output.outputowners.md).[threshold](common_output.outputowners.md#protected-threshold)*

*Defined in [src/common/output.ts:118](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L118)*

## Methods

### `Abstract` clone

▸ **clone**(): *this*

*Inherited from [Output](common_output.output.md).[clone](common_output.output.md#abstract-clone)*

*Defined in [src/common/output.ts:318](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L318)*

**Returns:** *this*

___

### `Abstract` create

▸ **create**(...`args`: any[]): *this*

*Inherited from [Output](common_output.output.md).[create](common_output.output.md#abstract-create)*

*Defined in [src/common/output.ts:320](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L320)*

**Parameters:**

Name | Type |
------ | ------ |
`...args` | any[] |

**Returns:** *this*

___

###  deserialize

▸ **deserialize**(`fields`: object, `encoding`: [SerializedEncoding](../modules/utils_serialization.md#serializedencoding)): *void*

*Overrides [OutputOwners](common_output.outputowners.md).[deserialize](common_output.outputowners.md#deserialize)*

*Defined in [src/common/output.ts:439](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L439)*

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`fields` | object | - |
`encoding` | [SerializedEncoding](../modules/utils_serialization.md#serializedencoding) | "hex" |

**Returns:** *void*

___

###  fromBuffer

▸ **fromBuffer**(`outbuff`: Buffer, `offset`: number): *number*

*Overrides [OutputOwners](common_output.outputowners.md).[fromBuffer](common_output.outputowners.md#frombuffer)*

*Defined in [src/common/output.ts:456](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L456)*

Popuates the instance from a [Buffer](https://github.com/feross/buffer) representing the [StandardAmountOutput](common_output.standardamountoutput.md) and returns the size of the output.

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`outbuff` | Buffer | - |
`offset` | number | 0 |

**Returns:** *number*

___

###  getAddress

▸ **getAddress**(`idx`: number): *Buffer*

*Inherited from [OutputOwners](common_output.outputowners.md).[getAddress](common_output.outputowners.md#getaddress)*

*Defined in [src/common/output.ts:167](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L167)*

Returns the address from the index provided.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`idx` | number | The index of the address.  |

**Returns:** *Buffer*

Returns the string representing the address.

___

###  getAddressIdx

▸ **getAddressIdx**(`address`: Buffer): *number*

*Inherited from [OutputOwners](common_output.outputowners.md).[getAddressIdx](common_output.outputowners.md#getaddressidx)*

*Defined in [src/common/output.ts:150](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L150)*

Returns the index of the address.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`address` | Buffer | A [Buffer](https://github.com/feross/buffer) of the address to look up to return its index.  |

**Returns:** *number*

The index of the address.

___

###  getAddresses

▸ **getAddresses**(): *Array‹Buffer›*

*Inherited from [OutputOwners](common_output.outputowners.md).[getAddresses](common_output.outputowners.md#getaddresses)*

*Defined in [src/common/output.ts:135](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L135)*

Returns an array of [Buffer](https://github.com/feross/buffer)s for the addresses.

**Returns:** *Array‹Buffer›*

___

###  getAmount

▸ **getAmount**(): *BN*

*Defined in [src/common/output.ts:451](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L451)*

Returns the amount as a [BN](https://github.com/indutny/bn.js/).

**Returns:** *BN*

___

###  getLocktime

▸ **getLocktime**(): *BN*

*Inherited from [OutputOwners](common_output.outputowners.md).[getLocktime](common_output.outputowners.md#getlocktime)*

*Defined in [src/common/output.ts:130](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L130)*

Returns the a [BN](https://github.com/indutny/bn.js/) repersenting the UNIX Timestamp when the lock is made available.

**Returns:** *BN*

___

### `Abstract` getOutputID

▸ **getOutputID**(): *number*

*Inherited from [Output](common_output.output.md).[getOutputID](common_output.output.md#abstract-getoutputid)*

*Defined in [src/common/output.ts:316](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L316)*

Returns the outputID for the output which tells parsers what type it is

**Returns:** *number*

___

###  getSpenders

▸ **getSpenders**(`addresses`: Array‹Buffer›, `asOf`: BN): *Array‹Buffer›*

*Inherited from [OutputOwners](common_output.outputowners.md).[getSpenders](common_output.outputowners.md#getspenders)*

*Defined in [src/common/output.ts:196](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L196)*

Given an array of addresses and an optional timestamp, select an array of address [Buffer](https://github.com/feross/buffer)s of qualified spenders for the output.

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`addresses` | Array‹Buffer› | - |
`asOf` | BN | undefined |

**Returns:** *Array‹Buffer›*

___

###  getThreshold

▸ **getThreshold**(): *number*

*Inherited from [OutputOwners](common_output.outputowners.md).[getThreshold](common_output.outputowners.md#getthreshold)*

*Defined in [src/common/output.ts:125](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L125)*

Returns the threshold of signers required to spend this output.

**Returns:** *number*

___

###  getTypeID

▸ **getTypeID**(): *number*

*Inherited from [Serializable](utils_serialization.serializable.md).[getTypeID](utils_serialization.serializable.md#gettypeid)*

*Defined in [src/utils/serialization.ts:52](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/utils/serialization.ts#L52)*

Used in serialization. Optional. TypeID is a number for the typeID of object being output.

**Returns:** *number*

___

###  getTypeName

▸ **getTypeName**(): *string*

*Inherited from [Serializable](utils_serialization.serializable.md).[getTypeName](utils_serialization.serializable.md#gettypename)*

*Defined in [src/utils/serialization.ts:45](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/utils/serialization.ts#L45)*

Used in serialization. TypeName is a string name for the type of object being output.

**Returns:** *string*

___

### `Abstract` makeTransferable

▸ **makeTransferable**(`assetID`: Buffer): *[StandardTransferableOutput](common_output.standardtransferableoutput.md)*

*Inherited from [Output](common_output.output.md).[makeTransferable](common_output.output.md#abstract-maketransferable)*

*Defined in [src/common/output.ts:330](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L330)*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`assetID` | Buffer | An assetID which is wrapped around the Buffer of the Output  Must be implemented to use the appropriate TransferableOutput for the VM.  |

**Returns:** *[StandardTransferableOutput](common_output.standardtransferableoutput.md)*

___

###  meetsThreshold

▸ **meetsThreshold**(`addresses`: Array‹Buffer›, `asOf`: BN): *boolean*

*Inherited from [OutputOwners](common_output.outputowners.md).[meetsThreshold](common_output.outputowners.md#meetsthreshold)*

*Defined in [src/common/output.ts:177](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L177)*

Given an array of address [Buffer](https://github.com/feross/buffer)s and an optional timestamp, returns true if the addresses meet the threshold required to spend the output.

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`addresses` | Array‹Buffer› | - |
`asOf` | BN | undefined |

**Returns:** *boolean*

___

### `Abstract` select

▸ **select**(`id`: number, ...`args`: any[]): *[Output](common_output.output.md)*

*Inherited from [Output](common_output.output.md).[select](common_output.output.md#abstract-select)*

*Defined in [src/common/output.ts:322](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L322)*

**Parameters:**

Name | Type |
------ | ------ |
`id` | number |
`...args` | any[] |

**Returns:** *[Output](common_output.output.md)*

___

###  serialize

▸ **serialize**(`encoding`: [SerializedEncoding](../modules/utils_serialization.md#serializedencoding)): *object*

*Overrides [OutputOwners](common_output.outputowners.md).[serialize](common_output.outputowners.md#serialize)*

*Defined in [src/common/output.ts:432](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L432)*

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`encoding` | [SerializedEncoding](../modules/utils_serialization.md#serializedencoding) | "hex" |

**Returns:** *object*

___

###  toBuffer

▸ **toBuffer**(): *Buffer*

*Overrides [OutputOwners](common_output.outputowners.md).[toBuffer](common_output.outputowners.md#tobuffer)*

*Defined in [src/common/output.ts:466](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L466)*

Returns the buffer representing the [StandardAmountOutput](common_output.standardamountoutput.md) instance.

**Returns:** *Buffer*

___

###  toString

▸ **toString**(): *string*

*Inherited from [OutputOwners](common_output.outputowners.md).[toString](common_output.outputowners.md#tostring)*

*Defined in [src/common/output.ts:261](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L261)*

Returns a base-58 string representing the [Output](common_output.output.md).

**Returns:** *string*

___

### `Static` comparator

▸ **comparator**(): *function*

*Inherited from [OutputOwners](common_output.outputowners.md).[comparator](common_output.outputowners.md#static-comparator)*

*Defined in [src/common/output.ts:265](https://github.com/ava-labs/avalanchejs/blob/2850ce5/src/common/output.ts#L265)*

**Returns:** *function*

▸ (`a`: [Output](common_output.output.md), `b`: [Output](common_output.output.md)): *0 | 1 | -1*

**Parameters:**

Name | Type |
------ | ------ |
`a` | [Output](common_output.output.md) |
`b` | [Output](common_output.output.md) |
