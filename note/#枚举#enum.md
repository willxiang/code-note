```
public enum CouponType {
    /**
     * 满减券
     */
    SATISFY_SUBTRACTION(1, "满减券"),

    /**
     * 立减券
     */
    SUBTRACTION(2, "立减券"),

    /**
     * 折扣券
     */
    DISCOUNT(3, "折扣券"),

    /**
     * 新人首充券
     */
    NEW_USER_FIRST_RECHARGE(4, "新人首充券"),

    /**
     * 充值折扣券
     */
    DISCOUNT_RECHARGE(5, "充值折扣券"),

    /**
     * 充值满送券
     */
    GIVEAWAY_RECHARGE(6, "充值满送券"),

    /**
     * 新用户邀请返利-现金
     */
    INVITE_NEW_USER_FOR_CASH(7, "新用户邀请返利-现金"),

    /**
     * 新用户邀请返利-印币
     */
    INVITE_NEW_USER_FOR_COIN(8, "新用户邀请返利-印币"),

    /**
     * 单次使用印币券
     */
    ONE_TIME_COIN(9, "单次使用印币券"),


    /**
     * 多次使用印币券
     */
    COIN(10, "多次使用印币券"),

    /**
     * 单次使用印数券
     */
    ONE_TIME_PRINT(11, "单次使用印数券"),


    /**
     * 多次使用印数券
     */
    PRINT(12, "多次使用印数券"),

    /**
     * 未知的券
     */
    UNKNOWN(999, "未知的券");


    private String desc;
    private Integer type;

    CouponType(Integer type, String desc) {
        this.type = type;
        this.desc = desc;
    }

    public static CouponType getCouponType(int type) {
        for (CouponType couponType : CouponType.values()) {
            if (couponType.getType() == type) {
                return couponType;
            }
        }
        return CouponType.UNKNOWN;
    }


    public Integer getType() {
        return this.type;
    }

    public String getDesc() {
        return this.desc;
    }


}

```