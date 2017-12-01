# fastq_analysis_tool
```
#! /bin/python/
# encoding = utf-8
"""
#A Tool Deal With FASTQ:
#	1.cut off 5',3' bases(input the num)
#	2.the length distribution of reads
#	3.FASTQ to FASTA
#	4.the number of bases and the ratio of GC
Author : Xiaoting Zhang
"""
from optparse import OptionParser
import sys
args = sys.argv

def cut_fastq(filename,cut_5=0,cut_3=0):
	"""
	cut off 5' and 3' bases
	:param:filename  -- the file which will be dealed with
	:param:cut_5  -- 5' bases number which will be cutted off
	:param:cut_3  -- 3' bases number which will be cutted off

	"""
	with open(filename) as f:
		while f:
			line_1 = f.readline().strip("\n")
			if not (line_1 and f):
				break
			line_2 = f.readline().strip("\n")
			line_3 = f.readline().strip("\n")
			line_4 = f.readline().strip("\n")
			cut_5 = int(cut_5)
			cut_3 = int(cut_3)
			line2 = line_2[cut_5:-cut_3]
			line4 = line_4[cut_5:-cut_3]
			print(line_1,"\n",line2,"\n",line_3,"\n",line4)
	f.close()
def reads_dis(filename):
	"""
	statistics the reads length distribution
	parma:filename -- a fastq format file which will be statistics
	
	"""
	read_dis = {}
	with open(filename) as f:
		while f:
			line_1 = f.readline().strip("\n")
			if not (line_1 and f):
				break
			line_2 = f.readline().strip("\n")
			line_3 = f.readline().strip("\n")
			line_4 = f.readline().strip("\n")
			length=len(line_2)
			if not length in read_dis:
				read_dis[length] = 0
			read_dis[length] += 1
	for length,num in read_dis.items():
		print("length : %s \t Number : %s" % (length,num))
	f.close()
def fastq2fasta(filename):
	"""
	Convert fastq to fasta
	parma:filename  -- input the FASTQ format file
	the line_1 is ID and the line_2 is bases 
	"""
	with open(filename) as f:
		while f:
			line_1 = f.readline().strip("\n")
			if not (line_1 and f):
				break
			line_2 = f.readline().strip("\n")
			line_3 = f.readline().strip("\n")
			line_4 = f.readline().strip("\n")
			print(">",line_1)
			print(line_2)
	f.close()
def length_of_fq(filename):
	"""
	Sum the bases to count the length
	parma:the fastq filename
	
	"""
	length=0
	with open(filename) as f:
		while f:
			line_1 = f.readline().strip("\n")
			if not (line_1 and f):
				break
			line_2 = f.readline().strip("\n")
			line_3 = f.readline().strip("\n")
			line_4 = f.readline().strip("\n")
			length += len(line_2)
		print(length)
		return length
	f.close()
def gc_ratio(filename):
	"""
	count the GC ratio 
	param:filename  -- the input FASTQ format file
	
	"""
	count_gc = 0
	with open(filename) as f:
		while f:
			line_1 = f.readline().strip("\n")
			if not (line_1 and f):
				break
			line_2 = f.readline().strip("\n").upper()
			line_3 = f.readline().strip("\n")
			line_4 = f.readline().strip("\n")
			count_gc += line_2.count("G")+line_2.count("C")
	f.close()
	ratio = 0
	total = length_of_fq(filename)
	ratio =( count_gc * 1.0 )/total
	print("GC : %s" % (ratio))

def main(args):
	parser = OptionParser()
	parser.add_option("-f","--fastq",dest="filename",help="fastq file",metavar="file")
	parser.add_option("--cut","--cut-fastq",dest="cut",help="cut fastq file",action="store_true",default=False)
	parser.add_option("-5","--cut-5",dest="cut_5",type=int,help="cut fastq N(N>0) bases from 5'",metavar="INT",default=0)
	parser.add_option("-3","--cut-3",dest="cut_3",type=int,help="cut fastq N(N>0) bases from 3'",metavar="INT",default=0)
	parser.add_option("--len-dis",dest="len_dis",help="sequence length distribution",action="store_true",default=False)
	parser.add_option("--fasta","--fastq2fasta",dest="fasta",help="convert fastq to fasta",action="store_true",default=False)
	parser.add_option("--gc","--count-gc",dest="gc",help="count the GC%",action="store_true",default=False)
	parser.add_option("--len","--fastq-length",dest="fq_len",help="total reads number",action="store_true",default=False)
	(options,args) = parser.parse_args()
	if not options.filename:
		parser.print_help()
	filename=options.filename
	if options.cut:
		cut_5 = options.cut_5
		cut_3 = options.cut_3
		cut_fastq(filename,cut_5,cut_3)
	if options.len_dis:
		reads_dis(filename)
	if options.fq_len:
		length_of_fq(filename)
	if options.fasta:
		fastq2fasta(filename)
	if options.gc:
		gc_ratio(filename)
if __name__ == '__main__':
	main(args)
```

