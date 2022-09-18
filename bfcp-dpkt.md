# bfcp dpkt

参考连接：

1. <https://blog.csdn.net/qq_41681715/article/details/109824079>
2. <https://www.rfc-editor.org/rfc/rfc8855.html#name-bfcp-primitives>
3. <https://datatracker.ietf.org/doc/html/rfc4582>
4. <https://www.wireshark.org/docs/dfref/b/bfcp.html>
5. <https://kbandla.github.io/dpkt/creating_parsers.html>


```python
import struct

import dpkt

# Value	Primitive
FloorRequest = 1
FloorRelease = 2
FloorRequestQuery = 3
FloorRequestStatus = 4
UserQuery = 5
UserStatus = 6
FloorQuery = 7
FloorStatus = 8
ChairAction = 9
ChairActionAck = 10
Hello = 11
HelloAck = 12
Error = 13
FloorRequestStatusAck = 14
FloorStatusAck = 15
Goodbye = 16
GoodbyeAck = 17

BENEFICIARY_ID = 1
FLOOR_ID = 2
FLOOR_REQUEST_ID = 3
Priority = 4
REQUEST_STATUS = 5
ERROR_CODE = 6
ERROR_INFO = 7
PARTICIPANT_PROVIDED_INFO = 8
STATUS_INFO = 9
SUPPORTED_ATTRIBUTES = 10
SUPPORTED_PRIMITIVES = 11
USER_DISPLAY_NAME = 12
USER_URI = 13
BENEFICIARY_INFORMATION = 14
FLOOR_REQUEST_INFORMATION = 15
REQUESTED_BY_INFORMATION = 16
FLOOR_REQUEST_STATUS = 17
OVERALL_REQUEST_STATUS = 18


class BFCP(dpkt.Packet):
    """Binary Floor Control Protocol."""
    """
    FIELD NAME                         DESCRIPTION					      TYPE						    VERSIONS
    bfcp.attribute_length              Attribute Length				      Unsigned integer, 2 bytes	    1.8.0 to 3.6.5
    bfcp.attribute_length.too_small    Attribute length is too small      Label						    1.12.0 to 3.6.5
    bfcp.attribute_type                Attribute Type				      Unsigned integer, 1 byte	    1.8.0 to 3.6.5
    bfcp.attribute_types_m_bit         Mandatory bit(M)				      Boolean					    1.10.0 to 3.6.5
    bfcp.beneficiary_id                BENEFICIARY-ID				      Unsigned integer, 2 bytes	    1.10.0 to 3.6.5
    bfcp.conference_id                 Conference ID				      Unsigned integer, 4 bytes	    1.8.0 to 3.6.5
    bfcp.error_code                    Error Code					      Unsigned integer, 1 byte	    1.10.0 to 3.6.5
    bfcp.error_info_text               Text							      Character string			    1.10.0 to 3.6.5
    bfcp.error_specific_details        Error Specific Details		      Sequence of bytes			    2.0.0 to 3.6.5
    bfcp.floor_id                      FLOOR-ID						      Unsigned integer, 2 bytes	    1.10.0 to 3.6.5
    bfcp.floorrequest_id               FLOOR-REQUEST-ID				      Unsigned integer, 2 bytes	    1.10.0 to 3.6.5
    bfcp.hdr_f_bit                     Fragmentation (F)			      Boolean					    1.10.0 to 3.6.5
    bfcp.hdr_r_bit                     Transaction Responder (R)	      Boolean					    1.10.0 to 3.6.5
    bfcp.padding                       Padding						      Sequence of bytes			    2.0.0 to 3.6.5
    bfcp.part_prov_info_text           Text							      Character string			    1.10.0 to 3.6.5
    bfcp.payload                       Payload						      Sequence of bytes			    1.8.0 to 3.6.5
    bfcp.payload_length                Payload Length				      Unsigned integer, 2 bytes	    1.8.0 to 3.6.5
    bfcp.primitive                     Primitive					      Unsigned integer, 1 byte	    1.8.0 to 3.6.5
    bfcp.priority                      FLOOR-REQUEST-ID				      Unsigned integer, 2 bytes	    1.10.0 to 3.6.5
    bfcp.queue_pos                     Queue Position				      Unsigned integer, 1 byte	    1.10.0 to 3.6.5
    bfcp.req_by_i                      Requested-by ID				      Unsigned integer, 2 bytes	    1.10.0 to 3.6.5
    bfcp.request_status                Request Status				      Unsigned integer, 1 byte	    1.8.0 to 3.6.5
    bfcp.status_info_text              Text							      Character string			    1.10.0 to 3.6.5
    bfcp.supp_attr                     Supported Attribute			      Unsigned integer, 1 byte	    1.10.0 to 3.6.5
    bfcp.supp_primitive                Supported Primitive			      Unsigned integer, 1 byte	    1.10.0 to 3.6.5
    bfcp.transaction_id                Transaction ID				      Unsigned integer, 2 bytes	    1.8.0 to 3.6.5
    bfcp.transaction_initiator         Transaction Initiator		      Boolean					    1.8.0 to 1.8.15
    bfcp.user_disp_name                Name							      Character string			    1.10.0 to 3.6.5
    bfcp.user_id                       User ID						      Unsigned integer, 2 bytes	    1.8.0 to 3.6.5
    bfcp.user_uri                      URI							      Character string			    1.10.0 to 3.6.5
    bfcp.ver                           Version(ver)					      Unsigned integer, 1 byte	    1.10.0 to 3.6.5
    """

    __hdr__ = (
        ('_v_f_r', 'B', 0x20),
        ('primitive', 'B', 0),
        ('len', 'H', 0),
        ('conf', 'I', 0),
        ('trans', 'H', 0),
        ('user', 'H', 0),
    )
    __bit_fields__ = {
        '_v_f_r': (
            ('ver', 3),
            ('hdr_r_bit', 1),
            ('hdr_f_bit', 1),
            ('res', 3),
        )
    }
    attr = {}

    def __init__(self, *args, **kwargs):
        super(BFCP, self).__init__(*args, **kwargs)

    def __len__(self):
        return int(len(self.data) / 4)

    def __bytes__(self):
        """"""
        self.len = self.__len__()
        return self.pack_hdr()

    def unpack(self, buf):
        dpkt.Packet.unpack(self, buf)
        length = int(len(self.data) / 4)
        if length != self.len:
            raise dpkt.UnpackError('invalid header length')
        if self.len:
            start = 0
            while start < len(self.data):
                end = self.data[start + 1]
                attr = self.data[start:start + end]
                start += end
                attr = BFCPAttr(attr)
                self.attr.setdefault(attr.type, attr)


class BFCPAttr(dpkt.Packet):
    """
      +------+---------------------------+---------------+
      | Type | Attribute                 | Format        |
      +------+---------------------------+---------------+
      |   1  | BENEFICIARY-ID            | Unsigned16    |
      |   2  | FLOOR-ID                  | Unsigned16    |
      |   3  | FLOOR-REQUEST-ID          | Unsigned16    |
      |   4  | PRIORITY                  | OctetString16 |
      |   5  | REQUEST-STATUS            | OctetString16 |
      |   6  | ERROR-CODE                | OctetString   |
      |   7  | ERROR-INFO                | OctetString   |
      |   8  | PARTICIPANT-PROVIDED-INFO | OctetString   |
      |   9  | STATUS-INFO               | OctetString   |
      |  10  | SUPPORTED-ATTRIBUTES      | OctetString   |
      |  11  | SUPPORTED-PRIMITIVES      | OctetString   |
      |  12  | USER-DISPLAY-NAME         | OctetString   |
      |  13  | USER-URI                  | OctetString   |
      |  14  | BENEFICIARY-INFORMATION   | Grouped       |
      |  15  | FLOOR-REQUEST-INFORMATION | Grouped       |
      |  16  | REQUESTED-BY-INFORMATION  | Grouped       |
      |  17  | FLOOR-REQUEST-STATUS      | Grouped       |
      |  18  | OVERALL-REQUEST-STATUS    | Grouped       |
      +------+---------------------------+---------------+
    """
    __hdr__ = (
        ('_attr', 'B', 1),
        ('len', 'B', 0)
    )
    __bit_fields__ = {
        '_attr': (
            ('type', 7),
            ('mandatory', 1)
        )
    }

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        if not args and 'len' not in kwargs:
            self.len = self.__len__()

    def __len__(self):
        return self.__hdr_len__ + len(self.data)

    def pack(self):
        if self.type == FLOOR_ID:
            self.len = 4
        return self.pack_hdr() + bytes(self.data)

    def unpack(self, buf):
        dpkt.Packet.unpack(self, buf)
        if len(buf) != self.len:
            raise dpkt.UnpackError('invalid header length')


class SupportedPrimitives(BFCPAttr):
    pass


class SupportedAttributes(BFCPAttr):
    pass


def test_bfcp():
    b = BFCP()
    d = b.pack_hdr()
    print(d)
    assert d == b'\x20\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'


def test_hello():
    b = BFCP()
    b.primitive = Hello
    b.len = 0
    b.conf = 16974375
    b.trans = 23353
    b.user = 552
    d = b.pack_hdr()
    assert d == b'\x20\x0b\x00\x00\x01\x03\x02\x27\x5b\x39\x02\x28'


def test_hello_ack():
    b = BFCP()
    b.primitive = Hello
    b.len = 0
    b.conf = 16974375
    b.trans = 23353
    b.user = 552
    d = b.pack_hdr()
    assert d == b'\x20\x0b\x00\x00\x01\x03\x02\x27\x5b\x39\x02\x28'


def test_unpack_hello_ack():
    s = b'\x20\x0b\x00\x06\x01\x03\x02\x27\x5b\x39\x02\x28\x17\x0c\x01\x02\x03\x04\x07\x08\x0b\x0c\x0e\x10\x15\x0c\x02\x03\x04\x05\x06\x0a\x0b\x0c\x0d\x0e'
    b = BFCP(s)
    assert b.conf == 16974375
    assert b.len == 6
    assert b.trans == 23353
    assert b.user == 552
    assert SUPPORTED_PRIMITIVES in b.attr
    assert b.attr[SUPPORTED_PRIMITIVES].len == 12
    assert [i for i in b.attr[SUPPORTED_PRIMITIVES].data] == [1, 2, 3, 4, 7, 8, 11, 12, 14, 16]
    assert SUPPORTED_ATTRIBUTES in b.attr
    assert b.attr[SUPPORTED_ATTRIBUTES].len == 12
    assert [i for i in b.attr[SUPPORTED_ATTRIBUTES].data] == [2, 3, 4, 5, 6, 10, 11, 12, 13, 14]


def test_floor_request():
    b = BFCP()
    b.conf = 16974375
    b.len = 1
    b.trans = 23353
    b.user = 552
    b.primitive = FloorRequest
    b.attr = BFCPAttr(type=FLOOR_ID, mandatory=1, data=1)
    d = b.pack()
    print(d)
    assert d == (b'\x20'
                 b'\x01'  # FloorRequest
                 b'\x00\x01'  # payload length
                 b'\x01\x03\x02\x27'  # conf
                 b'\x5b\x39'  # trans
                 b'\x02\x28'  # user
                 b'\x05'  # attr type 
                 b'\04'  # attr length 
                 b'\x00\x01')  # floor id


def test_attr_floor_id():
    attr = BFCPAttr(type=FLOOR_ID, mandatory=1, data=1)
    d = attr.pack()
    assert d == (b'\x05'  # attr type
                 b'\04'  # attr length
                 b'\x00\x01')


def test_hello_ack_primitives():
    s = b'\x17\x0c\x01\x02\x03\x04\x07\x08\x0b\x0c\x0e\x10'
    b = BFCPAttr(s)
    assert b.type == SUPPORTED_PRIMITIVES
    assert b.mandatory == 1
    assert b.len == 12
    assert [i for i in b.data] == [1, 2, 3, 4, 7, 8, 11, 12, 14, 16]


def test_hello_ack_attrs():
    s = b'\x15\x0c\x02\x03\x04\x05\x06\x0a\x0b\x0c\x0d\x0e'
    b = BFCPAttr(s)
    assert b.type == SUPPORTED_ATTRIBUTES
    assert b.mandatory == 1
    assert b.len == 12
    assert [i for i in b.data] == [2, 3, 4, 5, 6, 10, 11, 12, 13, 14]


def test_floor_request_status():
    pass

```

---

> [跳转到目录](index.md)
