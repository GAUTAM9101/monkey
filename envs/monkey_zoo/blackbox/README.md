# Automatic blackbox tests
### Prerequisites
1. Download google sdk: https://cloud.google.com/sdk/docs/
2. Download service account key for MonkeyZoo project (if you deployed MonkeyZoo via terraform scripts then you already have it). 
GCP console -> IAM -> service accounts(you can use the same key used to authenticate terraform scripts)
3. Deploy the relevant branch + complied executables to the Island machine on GCP.   

### Running the tests
In order to execute the entire test suite, you must know the external IP of the Island machine on GCP. You can find 
this information in the GCP Console `Compute Engine/VM Instances` under _External IP_. 

#### Running in command line
Run the following command:

`monkey\envs\monkey_zoo\blackbox>python -m pytest -s --island=35.207.152.72:5000 test_blackbox.py`

#### Running in PyCharm
Configure a PyTest configuration with the additional arguments `-s --island=35.207.152.72` on the 
`monkey\envs\monkey_zoo\blackbox`.

### Running telemetry performance test
To run telemetry performance test follow these steps:
1. Gather monkey telemetries.
    1. Enable "Export monkey telemetries" in Configuration -> Internal -> Tests if you don't have 
    exported telemetries already.
    2. Run monkey and wait until infection is done.
    3. All telemetries are gathered in `monkey/telem_sample`
2. Run telemetry performance test.
    1. Move directory `monkey/test_telems` to `envs/monkey_zoo/blackbox/tests/performance/test_telems`
    2. (Optional) Use `envs/monkey_zoo/blackbox/tests/performance/utils/telem_parser.py` to multiply 
    telemetries gathered.
        1. Run `telem_parser.py` script with working directory set to `monkey\envs\monkey_zoo\blackbox`
        2. Pass integer to indicate the multiplier. For example running `telem_parser.py 4` will replicate
        telemetries 4 times.
        3. If you're using pycharm check "Emulate terminal in output console" on debug/run configuraion.
    3. Performance test will run as part of BlackBox tests or you can run it separately by adding 
    `-k 'test_telem_performance'` option.
