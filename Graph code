import enum
from pyvis.network import Network
import grouping_code as gc
import random

net = Network()

def main(data):
    print('starting graph code')
    for num,node in enumerate(data):
        #Make node a random color for visual clarity
        random_number = random.randint(0,16777215)
        hex_number = str(hex(random_number))
        hex_number ='#'+ hex_number[2:]

        net.add_node(num, label='Person' + str(num),color=hex_number)
	
    for person,node in enumerate(data):
        for index,entry in enumerate(node):
            if entry:
                net.add_edge(person,index,value=entry)



    net.repulsion(node_distance=100, spring_length=200)
    net.show('nodes.html')

if __name__ == "__main__":
    main(gc.main())
