# CN2026 HW3

Sample code and environment for Assignment 3. Please read the assignment spec on NTU COOL first. If this README and the spec disagree, the spec is the reference.

## Repository structure

```
.
├── Dockerfile
├── docker-compose.yml
├── fsm.py                  # pseudo code of the sender / receiver FSM
├── README.md
├── hw3/                    # your code goes here (only this folder is submitted)
│   ├── sender.cpp          # sample code, implement your sender here
│   ├── receiver.cpp        # sample code, implement your receiver here
│   ├── agent.cpp           # given, do NOT modify (we use the original one when grading)
│   ├── crc32.cpp           # sample code for CRC-32
│   ├── sha256.cpp          # sample code for SHA-256
│   ├── def.h               # segment definition and constants
│   └── Makefile
└── log_checker/            # checks whether your logs fit the specification
    ├── README.md
    ├── log_spec.md         # log format
    ├── log_test.py         # main script
    ├── log_parser.py
    ├── ops.py
    └── config.py
```

## Environment

```bash
docker compose up -d        # or `docker-compose up -d`
docker ps -a                # find the container name
docker exec -it <container_name> bash
```

In the container, `hw3/` is mounted at `/hw3` and `log_checker/` is mounted at `/log_checker`.
You can write code on the host and compile / test in the container.
The container has no internet access by default (see the comments in `docker-compose.yml`).

## Build and run

```bash
cd /hw3
make
# in 3 terminals, in this order
./agent 8888 localhost 8887 localhost 8889 0.01 > agent_log.txt
./receiver localhost 8889 localhost 8888 ./output.bin > receiver_log.txt
./sender localhost 8887 localhost 8888 ./input.bin > sender_log.txt
```

Generate a random input file, e.g. `head -c 1000000 /dev/urandom > input.bin`.
Print only the specified logs to stdout. Use stderr for your own debug messages.

## Check your logs

```bash
cd /log_checker
python3 log_test.py <sender_log> <receiver_log> <agent_log> <src_filepath> <dst_filepath>
```

The autograde script used for grading is stricter than `log_test.py`, but if your logs cannot pass `log_test.py`, they will not pass the autograde script either. See `log_checker/README.md` for details.

## Submission

Compress your `hw3/` folder into `{studentID}_hw3.zip` (e.g. `B12902999_hw3.zip`) and upload it to NTU COOL:

```bash
zip -r B12902999_hw3.zip hw3 -x 'hw3/sender' 'hw3/receiver' 'hw3/agent' 'hw3/*.o'
```

After unzipping, there should be a `hw3/` folder that contains your `Makefile` and source code.
Executables are removed before grading, and we run `make` in `hw3/`.
