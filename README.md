# Geonames-embeddings
Embeddings for all geonames populated locations with population greater than 0

This project contains DeepWalk embeddings for populated places in Geonames. 
We describe the files first before giving a few examples. All embeddings were generated using the gensim package in Python.

FILES:

(1-7). populated-place-embeddings-{1-7}.jl.gz are compressed JSON lines files where each line is a JSON containing a single
key-value pair. For example:
{"<Presidio_3521133>": [-0.00016037290333770216, -0.16331960260868073, 0.14559058845043182, 0.014049071818590164, 0.016628244891762733, -0.1897198110818863, 0.14188186824321747, 0.11444853991270065, 0.018866628408432007, -0.04606882855296135, -0.15467466413974762, 0.18563739955425262, -0.053200043737888336, -0.16911742091178894, -0.05657876282930374, -0.00039542425656691194, 0.06017935276031494, 0.019657278433442116, 0.014765074476599693, 0.1783478558063507, -0.0005339447525329888, -0.10394854098558426, -0.17260663211345673, 0.022552259266376495, 0.060257066041231155, 0.298672616481781, 0.06802289932966232, -0.03377196565270424, 0.13092969357967377, 0.007119285874068737, -0.011325502768158913, -0.03717223182320595, 0.10660370439291, 0.09025651216506958, 0.023348767310380936, -0.1824614256620407, 0.13780122995376587, -0.03130345419049263, -0.10430310666561127, 0.02684781886637211, 0.06622970849275589, -0.11856741458177567, -0.053687382489442825, 0.14932486414909363, 0.03837030380964279, 0.0744427815079689, 0.08571769297122955, 0.007302495185285807, 0.11334840208292007, 0.0385720431804657, -0.15567876398563385, -0.134855255484581, 0.10208329558372498, 0.07373132556676865, -0.08734219521284103, 0.0397501066327095, -0.07437381893396378, 0.019372688606381416, -0.03451831266283989, 0.015789508819580078, 0.05273417755961418, -0.06393982470035553, -0.09630796313285828, -0.047562047839164734, -0.07217235118150711, -0.05011208727955818, 0.08369360864162445, 0.08336202055215836, 0.05594024807214737, 0.12139936536550522, 0.051332831382751465, 0.027806641533970833, 0.09555482864379883, -0.14043797552585602, -0.003694443963468075, 0.009580886922776699, 0.19412516057491302, 0.05366421863436699, 0.10408374667167664, 0.12925657629966736, -0.07961612194776535, -0.07847319543361664, -0.23122484982013702, -0.1003454178571701, 0.1332612931728363, -0.00953035056591034, 0.030777478590607643, 0.054927386343479156, -0.17683790624141693, 0.0793348103761673, 0.06144792586565018, -0.05212269350886345, 0.07661330699920654, 0.08598912507295609, -0.026763606816530228, 0.0698607936501503, 0.16680659353733063, 0.07281855493783951, -0.0883372575044632, 0.015735004097223282]}
is the 100-dimensional embedding of Presidio, which has Geonames ID 3521133. This way, each location is canonically
identified but is also human-readable.

To combine all the files into a single jl file use the cat command (after decompressing) in a UNIX-like terminal

The first 100 lines of populated-place-embeddings-1.jl.gz are in samples-100-locations.jl and can be viewed in a browser

8. mapped_places.txt.gz: To efficiently read things in memory and process, we mapped each of the mnemonic Geonames keys to an integer. E.g. 
<Presidio_3521133>	287145
we use tab to separate the two items. 100 sample lines were written out to samples-100-mapped_places.txt
Note that mapped_places.txt contains keys that you will not find in the embedding files. These are places that are not
populated anymore i.e. geonames shows them as having population 0 (or missing a population feature).

9. random-walk-corpus.txt.gz: contains the random walk corpus that was used by DeepWalk for the embeddings. Each line
contains a random walk sequence, space separated. Each item in the sequence is the mapped int of the mnemonic Geonames key.
We initiated 5 samples per node, up to a maximum depth of 10.

10. weighted_adjacency_list.tsv.gz: contains the directed weighted network that was used to generate the random walks. The
items in each line are tab-delimited and are of the form:
[node-out] [node-in1] [weight1] [node-in2] [weight2]...
where node-out is the node from which the edges are outgoing (to node-in1, node-in2...). The weights represent a probability
distribution that are inversely related to the distance between the places represented by the nodes. For more details,
see the paper. 

samples-100-weighted_adjacency_list.tsv contains the first 100 lines of (11) for easy viewing.

