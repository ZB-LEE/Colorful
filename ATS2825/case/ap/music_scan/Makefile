# **********************************************
# Type of Processor	& Board
# **********************************************
Processor =	mips
# **********************************************
# Directories
# **********************************************
#�ѹ���·�����ؽ���
include	./../../cfg/common_path
#�ѹ�������makefile���ؽ���
include	./../../cfg/rules.mk
# **********************************************
# Name of application ������Ա���޸Ĵ�(1)
IMAGENAME = mus_scan

#��Ҫ�����Դ�ļ��Ĵ��λ��,������Ա���޸Ĵ�(2)
SRC = $(CASE)/ap
SRCDIR_16 = $(SRC)/music_scan
SRCDIR_32 = $(SRCDIR_16)/O2
#Ŀ���ļ��������ļ���·��,������Ա���޸Ĵ�(3)
OBJECT_BIN_PATH = $(CASE)/fwpkg/ap
# **********************************************


#��������
DRV_BUILDER = $(PSP_REL)/tools/drv_builder.exe
AP_BUILDER = $(PSP_REL)/tools/ap_builder.exe
MMM_CODEC_BUILDER = $(PSP_REL)/tools/mmm_codec_builder.exe  
#��Ž���ļ���Ŀ¼�����������Ϊ��ǰĿ¼�������ɵĽ���ļ����ڵ�ǰĿ¼
LINKDIR	  =	.
#ָ���������̵��ļ�������·������Դ�ļ���·��д�ϼ���
VPATH	=	$(SRCDIR_16) $(SRCDIR_32)  $(SRC)/common/common_func/

# **********************************************
# Image	file names and map file
# **********************************************
#ָ������ļ�������
IMAGE_ELF =	$(IMAGENAME).exe
IMAGE_LST =	$(IMAGENAME).lst
IMAGE_REC =	$(IMAGENAME).rec
IMAGE_BIN =	$(IMAGENAME).sys
IMAGE_INFO = $(IMAGENAME).info
MAP		  =	$(IMAGENAME).map
TARGET_DRV = $(IMAGENAME).al
TARGET_EXT   = exe lst map ap info
TARGET_FILES = $(foreach n,$(TARGET_EXT),$(IMAGENAME).$(n))

# **********************************************
# Compiler and linker options
# **********************************************

#ָ�����������õ�ͷ�ļ�������·����һ��Դ��includeʱֻд��ͷ�ļ������ƣ�û��·����Ϣ����ʱ�͸������ѡ��ȥ������ͷ�ļ�
INCLUDE	  =	 -I$(PSP_REL)/include/ucos -I$(PSP_REL)/include -I$(CASE)/inc

#ָ������ʱ������·��
LINCLUDE =  -L$(PSPLIBDIR) -L$(PSPLIBDIR)/../include  

#ָ���Զ������ӽű�������
LD_SCRIPT = music_scan_linker.xn

#����������ѡ��
LD_OPTS = $(LINCLUDE) -G 0 -T $(LD_SCRIPT) -o $(IMAGE_ELF) -Map $(MAP) $(OFORMAT) 
LD_OPTS_1   = $(LINCLUDE) -G0 -T $(LD_SCRIPT) $(OFORMAT)
LD_OPTS_2   = -o $(IMAGE_ELF) -Map $(MAP)


# **********************************************
# Files	to be compiled
# **********************************************

#���.c��׺Դ��
SRC_C_16  = $(foreach dir,	$(SRCDIR_16), $(wildcard $(dir)/*.c))

SRC_C_16 += $(SRC)/common/common_func/common_share_data.c
#ת��Ϊ.o��ʽ�ļ����ƣ�����·����Ϣ
OBJ_C_16  = $(notdir $(patsubst %.c, %.o, $(SRC_C_16)))

#���.c��׺Դ��
SRC_C_32  = $(foreach dir,	$(SRCDIR_32), $(wildcard $(dir)/*.c))
#ת��Ϊ.o��ʽ�ļ����ƣ�����·����Ϣ
OBJ_C_32  = $(notdir $(patsubst %.c, %.o, $(SRC_C_32)))

#���.S��׺Դ��
SRC_S_16  = $(foreach dir,	$(SRCDIR_16), $(wildcard $(dir)/*.S))
#ת��Ϊ.o��ʽ�ļ����ƣ�����·����Ϣ
OBJ_S_16  = $(notdir $(patsubst %.S, %.o, $(SRC_S_16)))

#���.c��׺Դ��
SRC_S_32  = $(foreach dir,	$(SRCDIR_32), $(wildcard $(dir)/*.S))
#ת��Ϊ.o��ʽ�ļ����ƣ�����·����Ϣ
OBJ_S_32  = $(notdir $(patsubst %.S, %.o, $(SRC_S_32)))

#������е�.o�ļ�����
OBJ = $(OBJ_C_16) $(OBJ_S_16) $(OBJ_C_32) $(OBJ_S_32)


# **********************************************
# Rules
# **********************************************
#makeĿ��
.PHONY : all
.PHONY : pre_make

#dump��lst�ļ�
all : pre_make $(IMAGE_ELF)
	$(OBJDUMP) -D $(IMAGE_ELF) > $(IMAGE_LST)
	$(READELF) -a $(IMAGE_ELF) > $(IMAGE_INFO)
	$(MMM_CODEC_BUILDER) $(IMAGE_ELF) $(TARGET_DRV) MP3
	cp -f $(TARGET_DRV) $(OBJECT_BIN_PATH)
pre_make :
	$(RM) -f $(SRC)/common/common_func/common_share_data.o
#���ӹ���
$(IMAGE_ELF) : $(OBJ)
	$(LD) $(LD_OPTS_1) $(LD_OPTS_2)

#�������	
$(OBJ_C_16) : %.o : %.c
	$(CC) $(CC_OPTS_BASE_16) -Os -DMUSIC_SCAN_SHARE_DATA -o	$@ $<
	@echo
	
$(OBJ_S_16) : %.o : %.S
	$(CC) $(CC_OPTS_O2_A_16) -o     $@ $<
	@echo

$(OBJ_C_32) : %.o : %.c
	$(CC) $(CC_OPTS_O2) -o	        $@ $<
	@echo
	
$(OBJ_S_32) : %.o : %.S
	$(CC) $(CC_OPTS_O2_A) -o        $@ $<
	@echo
	


	
#ɾ������ļ�
.PHONY : clean

clean :
	rm -f $(LINKDIR)/*.o
	rm -f $(LINKDIR)/$(IMAGENAME).*

#ֻɾ�� target �ļ�������*.o���Ա���û�޸ĵ�Դ�ļ�ҲҪ�ر���
.PHONY : clean_target

clean_target :
	rm -f $(LINKDIR)/$(TARGET_FILES)
	rm -f size.txt
	