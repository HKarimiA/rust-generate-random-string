# rust-generate-random-string
Three way to generate randomly a string of ASCII characters in the range A-Z and a-z

## first way
```rust
// in Cargo.toml
// [dependencies]
// rand = "0.8.5"

fn main() {
    use rand::Rng;
    const CHARSET: &[u8] = b"ABCDEFGHIJKLMNOPQRSTUVWXYZ\
                            abcdefghijklmnopqrstuvwxyz";
    const STR_LEN: usize = 10;
    let mut rng = rand::thread_rng();

    let rand_str: String = (0..STR_LEN)
        .map(|_| {
            let idx = rng.gen_range(0..CHARSET.len());
            CHARSET[idx] as char
        })
        .collect();
    println!("{}", rand_str);
}
```

## second way
```rust
fn main() {
    use rand::Rng;
    const CHARSET: [char; 52] = [
        'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R',
        'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j',
        'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z',
    ];
    const STR_LEN: usize = 10;
    let mut rng = rand::thread_rng();

    let mut rand_str = String::with_capacity(STR_LEN);
    for _c in 0..STR_LEN {
        rand_str.push(CHARSET[rng.gen_range(0..CHARSET.len())]);
    }

    println!("{}", rand_str);
}
```

## third way (fastest way)
```rust
fn main() {
    use rand::Rng;
    const STR_LEN: usize = 10;
    let mut rng = rand::thread_rng();

    let mut chars: Vec<u8> = vec![0; STR_LEN];
    for el in &mut chars {
        let mut ch = rng.gen_range(65..117);
        if ch > 90 {
            ch += 6;
        }
        *el = ch;
    }

    let rand_str = String::from_utf8(chars).unwrap();
    println!("{}", rand_str);
}
```
