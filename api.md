# Libsecp256k1 APIs
The `SECP256K1_API` macro is included before the function (i.e the API) declaration. This macro exposes these function to the code that includes libsecp256k1.

If `SECP256K1_API` is not present, then these functions will not be visible to the code that uses libsecp256k1. Since, libsecp256k1 is compiled with `-fvisibility=hidden` flag which hides all the global variable, global functions in the final binary.

The APIs mentioned below will have the `SECP256K1_API` macro prepended to it in the source code.

## Context
1. `extern const secp256k1_context *secp256k1_context_no_precomp`
   - A simple secp256k1 context object with no precomputed tables
   - This is variable not function
  
2. secp256k1_context* secp256k1_context_create(
    unsigned int flags
    )
    - Returns a pointer to newly created context object
  
3. secp256k1_context* secp256k1_context_clone(
    const secp256k1_context* ctx
    )
    - Copy a secp256k1 context object (into dynamically allocated memory)
  
4. void secp256k1_context_destroy(
    secp256k1_context* ctx
    )
    - Destroy a secp256k1 context object at the memory location given by the context pointer.
  
5. void secp256k1_context_set_illegal_callback(
    secp256k1_context* ctx,
    void (*fun)(const char* message, void* data),
    const void* data
    )
    - Set a callback function to be called when an illegal argument is passed to an API call.
  
6. void secp256k1_context_set_error_callback(
    secp256k1_context* ctx,
    void (*fun)(const char* message, void* data),
    const void* data
    )
    - Set a callback function to be called when an internal consistency check fails.
    - Default is crashing (if this api is not set).

## Scratch Space
1. secp256k1_scratch_space* secp256k1_scratch_space_create(
    const secp256k1_context* ctx,
    size_t size
    )
    - Creates a scratch space object.
    - This is needed to avoid dynamic memory allocation in internal APIs??

2. void secp256k1_scratch_space_destroy(
    const secp256k1_context* ctx,
    secp256k1_scratch_space* scratch
    )
    - Destroy the given scratch space object.

## Public Key (an Elliptic Curve Point)
1. int secp256k1_ec_pubkey_parse(
    const secp256k1_context* ctx,
    secp256k1_pubkey* pubkey,
    const unsigned char *input,
    size_t inputlen
    ) 
    - Creates a public key object (defined in libsecp) from the given encoded public key format.
    - Returns 1 on success else 0
    - Encoded pubkey format can be:
      - 33 bytes--header byte (0x02,0x03) + 32 bytes (compressed pubkey)
      - 65 bytes--header byte (0x04, 0x06, 0x07) + 64 bytes (uncompressed or hybrid pubkey)

2. int secp256k1_ec_pubkey_serialize(
    const secp256k1_context* ctx,
    unsigned char *output,
    size_t *outputlen, //TODO: why is outputlen a pointer here?
    const secp256k1_pubkey* pubkey,
    unsigned int flags
    )
    - Encodes the given pubkey object
  
3. int secp256k1_ec_pubkey_cmp(
    const secp256k1_context* ctx,
    const secp256k1_pubkey* pubkey1,
    const secp256k1_pubkey* pubkey2
    )
    - compare two pubkeys using lexographic (of compressed serialization) order
    - Returns:
      - 0 if both are equal
      - < 0 if first pubkey is less than second 
      - > 0 if first pubkey is greater that second

## Secret Key (a field element of Zp)

## ECDSA 