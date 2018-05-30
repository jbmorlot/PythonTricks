from multiprocessing import Process, Pipe
from time import sleep

def f(send_end,process_number,a,b):
    try:
        print "starting thread: ", process_number
        c = a + b
        sleep(10)
        send_end.send(c)
    
    except KeyboardInterrupt:
        send_end.send(c)
        print "Keyboard interrupt in process: ", process_number


processes = []

manager = Manager()
a = {i:i for i in range(4)}
b = {i:i for i in range(4)}

recv_end_c, send_end_c = zip(*[Pipe(False) for k in range(4)])

for i in range(4):
    recv_end, send_end = Pipe(False)
    p = Process(target=f, args=(send_end_c[i],i,a[i],b[i]))
    processes.append(p)
    p.start()

print 'join'
try:
    for process in processes:
        process.join()
        
    result_list = [x.recv() for x in recv_end_c]
        
except KeyboardInterrupt:
    print "Keyboard interrupt in main"
    result_list = [x.recv() for x in recv_end_c]

