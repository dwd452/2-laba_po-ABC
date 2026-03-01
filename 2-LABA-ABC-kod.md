import struct

#######################################################################
def f2b(x):
    return struct.unpack(">I", struct.pack(">f", x))[0]
def b2f(x):
    return struct.unpack(">f", struct.pack(">I", x))[0]
def slozhenie(a,b):
    ba=f2b(a)
    bb=f2b(b)
#######################################################################
    sa, ea, ma = (ba >> 31) & 1, (ba >> 23) & 255, ba & 0x7FFFFF
    sb, eb, mb = (bb >> 31) & 1, (bb >> 23) & 255, bb & 0x7FFFFF

    if ea!=0:
        ma|=1<<23
    if eb!=0:
        mb|=1<<23

    if ea>eb:
        mb>>=ea-eb
        e=ea
    else:
        ma>>=eb-ea
        e=eb

    if sa:
        ma=-ma
    if sb:
        mb=-mb

    m=ma+mb
    s=0
    if m<0:
        s=1
        m=-m
    while m>=(1 << 24):
        m>>=1
        e+=1
    while m and m<(1<<23):
        m<<=1
        e-=1

    m&=0x7FFFFF
    return b2f((s << 31) | (e << 23) | m)
#ниже пишем что хотим сложить(как сделать так чтобы пользователь сам вводил я не понял)
print(slozhenie(1.5, 2.25))
