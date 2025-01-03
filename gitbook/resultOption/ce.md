# Result Computation Expression

Namespace: `FsToolkit.ErrorHandling`

## Examples

### Example 1

```fsharp
let add x y z = x + y + z

let addResult : Result<int option, string> = resultOption {
  let! x = Ok (Some 30)
  let! y = Ok (Some 10)
  let! z = Ok (Some 2)
  return add x y z
}
// Ok (Some 42)
```

### Example 2

```fsharp
let add x y z = x + y + z

let addResult : Result<int option, string> = resultOption {
  let! x = Ok (Some 30)
  let! y = Error "Oops 1"
  let! z = Error "Oops 2"
  return add x y z
}
// Error "Oops 1"
```

### Example 3

The [ResultOption.map2 example](../resultOption/map2.md#example-2) can be written using the `resultOption` computation expression as below

```fsharp
// CreatePostRequestDto -> Result<CreatePostRequest, string>
let toCreatePostRequest (dto : CreatePostRequestDto) = 

  // Result<Location option, string>
  let locationR = resultOption {
    let! lat = 
      dto.Latitude
      |> Option.traverseResult Latitude.TryCreate 
    let! lng = 
      dto.Longitude
      |> Option.traverseResult Longitude.TryCreate
    return location lat lng
  }
  
  // Result<Tweet, string>
  let tweetR = Tweet.TryCreate dto.Tweet

  // Result<CreatePostRequest, string>
  Result.map2 createPostRequest2 tweetR locationR
```
