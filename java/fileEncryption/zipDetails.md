# Zip file details
The Zip format was designed for efficient extraction operations. As a result, the layout places "summary" headers about the compressed content before the content itself. The tradeoff is that it is necessary to seek backwards in a data stream during archive creation

## Entry names and paths
Every entry in a Zip is stored using an absolute path relative to the base of the archive.
Zip archives cannot have multiple entries with the same name.