### @RandyMcMillan Deliverables:

--

git clone, install libs and make 

```shell	

if ! hash brew 2>/dev/null; then \
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" && \
if ! hash make 2>/dev/null; then \
brew install make
fi
if ! hash curl 2>/dev/null; then \
brew install curl
fi
else
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/RandyMcMillan/bitcoin/randymcmillan-deliverables/./github/workflows/install-qt5.sh)" && \
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/RandyMcMillan/bitcoin/randymcmillan-deliverables/./github/workflows/install-bitcoin-libs.sh)" 
fi


```

</p>
</details>  

---

<details>
<summary>next step</summary>
<p>

```

insert code sample


```

</p>
</details>  


