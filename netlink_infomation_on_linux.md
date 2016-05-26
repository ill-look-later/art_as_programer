# Netlink Infomation On Linux




```CPP
int m_sock;
struct sockaddr_nl l_local;
QTcpSocket m_mySocket;
/ open netlink socket */
m_sock = socket(AF_NETLINK, SOCK_DGRAM, NETLINK_CONNECTOR);
if (m_sock == -1) {
    qDebug ("Socket problem");
    return;
}
l_local.nl_family = AF_NETLINK;
l_local.nl_groups = 23;
l_local.nl_pid    = getpid();
if (bind(m_sock, (struct sockaddr *)&l_local, sizeof(struct sockaddr_nl)) == -1) {
    qDebug ("Bind problem");
    close(m_sock);
    m_sock = 0;
    return;
}

m_mySocket = new QTcpSocket();
if (!m_mySocket->setSocketDescriptor(m_sock))
{
    qDebug ("Failed to assign native socket to QTcpSocket");
    close(m_sock);
    m_sock = 0;
    return;
}

connect (m_mySocket, SIGNAL(readyRead()), this, SLOT(netlinkDataAvailable()));@
```
