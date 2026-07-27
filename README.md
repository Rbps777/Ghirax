def calculate_hash(data: bytes) -> int:
    v3 = 0
    for b in data:
        v2 = b
        v3 = (v2 + ((v3 << 19) | (v3 >> 13))) & 0xFFFFFFFF 
    return (v3 ^ 0xCD7840B0) & 0xFFFFFFFF

def main():
    apiData = {
        "kernel32.dll" : [""],
        "ntdll.dll" : [""],
        "user32.dll" : [""],
        "advapi32.dll" : [""],
    }
    target = [] # hex value
    found = {}
    for hash in target:
        for library, apiList in apiData.items():
            for api in apiList:
                if not api:
                    continue
                apiHash = calculate_hash(api.encode('utf-8'))
                if (hash == apiHash):
                    found[hash] = f"{library}!{api}"
    for hash, api in found.items():
        print(f"{hex(hash)} : {api}")
if __name__ == "__main__":
    main()
